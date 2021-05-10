# Angular CLI Stuff

## Create Multiple Components 

+ Windows - `for %n in (comp1, comp2) do ng g c %n --module=app.module.ts`

## Deploy Angular Project To Github Pages

1. Add this `angular-cli-ghpages` module to project - `ng add angular-cli-ghpages`

2. Run this command - `ng deploy --base-href=/<repositoryname>/`

Done