# Angular CLI Stuff

## Create Multiple Components 

+ Windows - `for %n in (comp1, comp2) do ng g c %n --module=app.module.ts`

## Deploy Angular Project To Github Pages

1. Add this `angular-cli-ghpages` module to project - `ng add angular-cli-ghpages`

2. Run this command - `ng deploy --base-href=/<repositoryname>/`

3. Your Project URL will be like this - `https://username.github.io/reponame/`

Click [Here](https://github.com/angular-schule/angular-cli-ghpages) for more information
