# Angular Stuff

> **Install Angular**

+ `npm i g @angular/cli`

<br/>

> **Create Angular App**

+ `ng new APP-NAME`

<br/>

> **Run App**

+ `ng serve --open`

<br/>

---

> **Generate Angular Module**

+ `ng g m moduleName`

<br/>

> **Generate Angular Component**

+ `ng g c CompName`

<br/>

> **Generate Angular Component inside of a module**

+ `ng g c ModuleName/CompName`

<br/>

> **Generate Angular Service**

+ `ng g s serviceName`

<br/>

[More About `ng generate`](https://angular.io/cli/generate#ng-generate)

<br/>

---

> **Install Bootstrap to Angular**

+ `ng add @ng-bootstrap/ng-bootstrap`

<br/>

> **Install Material to Angular**

+ `ng add @angular/material`

<br/>

---

## If condition

```html
<div *ngIf="condition; then thenBlock else elseBlock"></div>
<ng-template #thenBlock>Content to render when condition is true.</ng-template>
<ng-template #elseBlock>Content to render when condition is false.</ng-template>
```

```html
<ng-template [ngIf]="condition">
    <div>Content to render when condition is true.</div>
</ng-template>
```

<br/>
<br/>

## Switch Case

```html
<container-element [ngSwitch]="switch_expression">
  <some-element *ngSwitchCase="match_expression_1">...</some-element>
  <some-element *ngSwitchCase="match_expression_2">...</some-element>
  ...
  <some-element *ngSwitchDefault>...</some-element>
</container-element>
```

<br/>
<br/>

## Angular Material - Position Center

```TypeScript
//app.module.ts file
import { FlexLayoutModule } from '@angular/flex-layout';
imports: [ FlexLayoutModule ]

// put this in element to center
fxLayout="column" fxLayoutAlign="space-around center"
```

<br/>
<br/>

## Get params from url

```TypeScript
//in the component.ts file
// put ActivatedRoute into imports list like this
import { ActivatedRoute } from '@angular/route';
imports: [ ActivatedRoute ]

// Usage
constructor(private router:ActivatedRoute){}
let data = this.router.snapshot.params.YOUR_URL_PARAM
```

<br/>
<br/>

---

## ngClass Directive

Add CSS class to HTML element if the condition true.

Syntax : `<element [ngClass]="{className: condition}"></element>`

Example

```html
<h1 [ngClass]="{myClass: data!=null}"></h1>
```

---

<br/>
<br/>

## Use Angular Material Dialog

```TypeScript
// app.module.ts
entryComponents: [ YourDialogComponent ]

// In YourDialogComponent.ts file
import { MatDialogRef } from '@angular/material/dialog';

export class EditDialogComponent implements OnInit {
  constructor(
    private dialog: MatDialogRef<EditDialogComponent>
  ) {}

// In parent component
import { MatDialog } from '@angular/material/dialog';
import { YourDialogComponent } from './path/to/component';

export class ParentComponent implements OnInit {
  constructor(
    public dialog: MatDialog,
  ) {}

  openDialog(): void {
    this.dialog
      .open(YourDialogComponent)
      .afterClosed()
      .subscribe()
  }
}
```

<br/>
<br/>

## Form Validation

TypeScript File

```TypeScript
  constructor(private formBuilder: FormBuilder) { }

  newUserForm = this.formBuilder.group({
    email: new FormControl('', [Validators.email, Validators.required,Validators.minLength(3)]),
    username: new FormControl('', Validators.required),
    password: new FormControl('', [Validators.required, Validators.min(3),Validators.maxLength(5)]),
    confirmPassword: new FormControl('', [Validators.required, Validators.max(3),Validators.pattern('[a-zA-Z ]*')])
  },{validators : checkPasswords()});
  
// Custom Validator
export function checkPasswords() {
  return (group: AbstractControl): ValidationErrors | null => {
    let password:any = group.get('password');
    let passwordConfirm:any = group.get('confirmPassword');

    if (password.value !== passwordConfirm.value && password.value !== undefined) {
      return passwordConfirm.setErrors({ notEquivalent: true });
    } else {
      return null;
    }
  };
}
```

HTML File

```HTML
 <form [formGroup]="newUserForm" (ngSubmit)="register()" #f="ngForm" ngNativeValidate>

        <p>Please enter your detail</p>

        <mat-form-field>
          <input type="text" matInput placeholder="Username" formControlName="username" required/>          
        </mat-form-field>
        <mat-form-field>
          <input type="email" matInput placeholder="Email" formControlName="email" required/>
        </mat-form-field>
        <mat-form-field>
          <input matInput type="password" placeholder="Password" formControlName="password" required/>
        </mat-form-field>
        <mat-form-field>
          <input matInput type="password" placeholder="Confirm Password" formControlName="confirmPassword" required/>
        </mat-form-field>

        <div class="error" *ngIf="f.submitted && isSignUpFailed">
          {{errorMsg}}
        </div>

        <br />

        <button mat-raised-button type="submit" color="primary" class="login-btn" [disabled]="!newUserForm.valid">
          Register
        </button>
        <p style="margin-top: 10px">
          Already have an account ? <a routerLink="/login">Login</a>
        </p>
      </form
<!--
  'ngNativeValidate' attribute need to validation
  '#f' is like an ID
  "Register" Button disabled until validation pass
  [formGroup] is used to get data to TypeScript File
-->
```

<br/>
<br/>

## Router Guards

Use to protect routes. 5 Types of router guards

+ CanActivate
+ CanDeactivate
+ CanLoad
+ Resolve
+ CanActivateChild

> Generate Router Guard

+ `ng g guard guardName`

```TypeScript
export class MyGuard implements CanActivate {
  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {
      // your  logic goes here
  }
}
```

Usage in routing module

```TypeScript
{
  path: '/login',
  component: LoginComponent,
  canActivate: [MyGuard],
}
```

---

**When dealing with http requests,**

```TypeScript
// In app.module.ts
import { HttpClientModule } from '@angular/common/http';
imports:[HttpClientModule]

// In other ts files
import { HttpClient } from '@angular/common/http';
```

---

<br>
<br>

## Authorization Using Http Interceptor

```TypeScript
import { Injectable } from '@angular/core';
import { HttpRequest, HttpHandler } from '@angular/common/http';
import { HttpEvent, HttpInterceptor } from '@angular/common/http';
import { HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, switchMap, tap } from 'rxjs/operators';

//  This service takes care about token saving, removing stuff
import { TokenStorageService } from '../services/token-storage.service';
// This service takes care about login, logout and sign up
import { AuthService } from '../services/auth.service';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  constructor(private tokenStorage: TokenStorageService, private auth: AuthService) {}
  
  // API route for token refresh
  private refreshURL = 'http://127.0.0.1:3000/auth/refresh';

  // intercept is a built in method. We write our logic inside of this method
  intercept(request: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const ACCESS_TOKEN = this.tokenStorage.getAccessToken();
    if (ACCESS_TOKEN) {
      request = this.addToken(request, ACCESS_TOKEN);
    }
    return next.handle(request).pipe(     
      catchError((err) => {
        // If refresh token error occurred, this block will do a logout 
        if (err.status == 401 && err instanceof HttpErrorResponse && err.url == this.refreshURL) {
          this.auth
            .logout()
            .toPromise()
            .then(() => console.log('Logout Succeeded'))
            .catch((er) => console.log(er));
          return throwError(err);
        }

        // When Access Token Expired, This block will get new access token 
        else if (err.status == 401 && err instanceof HttpErrorResponse) {
          const REFRESH_TOKEN = this.tokenStorage.getRefreshToken();

          // If there is no refresh token, this block will do a logout
          if (!REFRESH_TOKEN) {
            this.auth
              .logout()
              .toPromise()
              .then(() => console.log('Logout Succeeded'))
              .catch((er) => console.log(er));
            return throwError(err);
          } else {

            // Get new refresh token and send the request with it
            return this.auth.tokenRefresh().pipe(
              tap((token) => {
                // Save new access token in session storage
                this.tokenStorage.saveAccessToken(token.jwt);
              }),
              switchMap((token) => {
                // Attach new access token to request and send it to server
                const newReq = request.clone({
                  headers: request.headers.set(
                    'Authorization',
                    `Bearer ${token.jwt}`
                  ),
                });
                return next.handle(newReq);
              })
            );
          }
        }
        return throwError(err);
      })
    );
  }


  //This Method Attach Access Token To Outgoing Http Requests and return a cloned request with attached header
  private addToken(req: HttpRequest<any>, token: string | null): HttpRequest<any> {
    return req.clone({
      headers: req.headers.set('Authorization', `Bearer ${token}`)
    });
  }
}

```

<br/>
<br/>

## Adding Sort To Material Data Table

+ Add `matSort` attribute to `<table>` tag
+ Add `mat-sort-header` attribute to every `<th>` element that you want to sort

```TypeScript
//app.module.ts
import { MatSortModule } from '@angular/material/sort';
imports:[MatSortModule]

// component.ts
import { MatSort } from '@angular/material/sort';
import { ViewChild } from '@angular/core';
import { MatTableDataSource } from '@angular/material/table';

export class YourComponent implements OnInit {
  ...
  dataSource!: MatTableDataSource<any>;

  @ViewChild(MatSort) sort!: MatSort;
  
  ngOnInit(): void {}

  getData(): void {  
    this.facilitiesService
      .listData()
      .toPromise()
      .then((data: Facility[]) => {
        let array = data.map((fac) => {return { ...fac }});        
        this.recordCount = array.length;
        this.dataSource = new MatTableDataSource(array);
        this.dataSource.sort = this.sort;        
      })
      .catch((err) => console.log(err));
  ...
}
```
