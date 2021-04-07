# NgRx State Management

## File Structure 
+ **state.ts** - Initial state and state interface
+ **actions.ts** - All Actions
+ **reducer.ts** - Reducer function
+ **effects.ts** - All effects. (@ngrx/effects module required)
+ **selectors.ts** - All selectors

*Use Redux DevTools browser extention for understand behind the scens. `@ngrx/store-devtools` are required.*
*[Here](https://www.youtube.com/playlist?list=PL_euSNU_eLbdg0gKbR8zmVJb4xLgHR7BX) is a YouTube Playlist. [GitHub](https://github.com/leelanarasimha/ngrx-counter) repo for the playlist*

**!! [issue 1] &nbsp; By using `Store.select()` method, it will refresh all `store.select()` methods in the code, if it related to changes or not !!**
*[issue 1] YouTube Video Link - https://www.youtube.com/watch?v=8zyyfb9_lEI&list=PL_euSNU_eLbdg0gKbR8zmVJb4xLgHR7BX&index=8*

## app.module.ts
```TypeScript
import { StoreDevtoolsModule } from '@ngrx/store-devtools';
import { StoreModule } from '@ngrx/store';
import { EffectsModule } from '@ngrx/effects';
import { reducerFunction } from './path/to/reducer.ts';
import { myEffects } from './path/to/effects.ts';

imports: [
    StoreModule.forRoot({ anyName: reducerFunction }),
    EffectsModule.forRoot([myEffects]),
    StoreDevtoolsModule.instrument({ maxAge: 25, logOnly: environment.production })
]
```
## Model.ts 
```TypeScript
export interface CounterState {
    value: boolean
}

export interface Post {
    id: number,
    title: string,
    body: string
}

export interface PostState {
    postslist: Post[]
}

export interface AppState {
    counter: CounterState,
    posts: PostState
}
```

## State.ts
```TypeScript
// State with single section
import { MyState } from './path/to/model.ts';

export initialState: MyState = {
    title: 'First State',
    id: 0,
    isPublished: true
};
```
*When you have multiple states, create Root AppState and combine all states to AppState. Also this can do to reducers. Then in app module, use the root reducer object. Here is a youtube video - https://www.youtube.com/watch?v=wCvJ5uc-Pbo&list=PL_euSNU_eLbdg0gKbR8zmVJb4xLgHR7BX&index=13*

## Actions.ts
```TypeScript
import { createAction, props } from '@ngrx/store';
export const loadAction = createAction('[My Component] Load Data');
export const successAction = createAction('success');

// Action with payload
export const propAction = createAction(
    'Action With Data',
    // Syntax: props<{ data1: DataType, data2: DataType, data3: DataType,.... }>
    props<{ value: number }>()
);
...
```

## Reducer.ts
```TypeScript
import { createReducer, on, Action } from '@ngrx/store';
import { loadAction, successAction, propAction  } from './path/to/actions.ts';
import { initialState, MyState } from './path/to/state.ts';

const _reducer = createReducer(
    initialState,
    on(loadAction, (state: MyState, action: Action) => {
        // 'state' is the 'initialState' 
        return state;
    }),
    on(propAction , (state, action) => {
    // console.log(action)
        return {
            ...state,
            id: state.id + action.value
        }
    })
);

export function reducerFunction(state: MyState, action: Action) {
    return _reducer(state, action);
}
```

## In Component.ts file
```TypeScript
import { Store } from '@ngrx/store';
import { loadAction, successAction, propAction } from './path/to/actions.ts';
import { initialState, MyState } from './path/to/state.ts';
import { Observable, Subscription } from 'rxjs';

export class MyComponent implements OnInit {
    myValue: number = 7;
    data$: Observable<MyState>;
    dataSubscriptions: Subscription;
    constructor (private store: Store<{ nameInAppModule: MyState }>) { }
    
    ngOnInit(): void {
        this.dataSubscriptions = this.store.select('nameInAppModule').subscibe((data) => {
            this.data$ = data;
        });
        this.store.dispatch(loadAction());
    }
    
    someMethod(): void {
        // The 'value' word must be similar to word in propAction's prop<{ value: DataType }>
        this.store.dispatch(propAction({ value: this.myValue }));
    }
    
    ngOnDestroy(): void {
        this.dataSubscriptions.unsubscribe();
    }
}
```

## Selectors.ts &nbsp;&nbsp;-&nbsp;&nbsp;&nbsp;(Solution for [issue 1])
*Youtube Video Link - https://www.youtube.com/watch?v=U3Fw9xAKCT0&list=PL_euSNU_eLbdg0gKbR8zmVJb4xLgHR7BX&index=9*
```TypeScript
import { createSelector, createFeatureSelector } from '@ngrx/store';
import { MyState } from './path/to/state.ts';

// 'anyName' must be similar to name in app.module.ts StoreModule.forRoot({ anyName: reducerFunction })
const myFeatureSelector = createFeatureSelector<MyState>('anyName');
export const getTitle = createSelector(myFeatureSelector, state => state.title);
export const getID = createSelector(myFeatureSelector, state => state.id);
```
**component.ts file [issue 1] fixed**
```TypeScript
import { Store } from '@ngrx/store';
import { getTitle, getID } from './path/to/selector.ts'
import { loadAction, propAction } from './path/to/actions.ts';
import { initialState, MyState } from './path/to/state.ts';
import { Observable, Subscription } from 'rxjs';

export class MyComponent implements OnInit {
    myValue: number = 7;
    myTitle: string;
    idN: number;
    dataSubscriptions: Subscription;
    constructor (private store: Store<{ nameInAppModule: MyState }>) { }
    
    ngOnInit(): void {
        this.dataSubscriptions = this.store.select(getTitle).subscibe((data) => {
            this.myTitle = data;
        });
        this.store.dispatch(loadAction());
    }
    
    someMethod(): void {
        // The 'value' word must be similar to word in propAction's prop<{ value: DataType }>
        this.store.dispatch(propAction({ value: this.myValue }));
    }
    
    ngOnDestroy(): void {
        this.dataSubscriptions.unsubscribe();
    }
}
```

## Effects.ts

```TypeScript
import { loadPostsAction, loadPostsSuccessAction } from './posts.actions';
import { createEffect, Actions, ofType } from '@ngrx/effects';
import { PostService } from '../services/post.service';
import { Injectable } from '@angular/core';
import { map, mergeMap } from 'rxjs/operators';

@Injectable()
export class PostEffects {
  constructor(private postService: PostService, private actions$: Actions) {}

  loadPosts$ = createEffect(() => {
    return this.actions$.pipe(
      ofType(loadPostsAction), // loadPostsAction will be dispatched in somewhare
      mergeMap((action) => {
        return this.postService.getPosts().pipe(
          map((posts) => {
            console.log(posts);
            return loadPostsSuccessAction({ posts });
          })
        );
      })
    );
  });
}

```






