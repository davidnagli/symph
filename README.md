# Symph
Symph is a tool to orchestrate your development tools

The idea behind Symph is to make a really simple lightweight tool that reuses the same filewatcher and creates a nice and neat interface to manage all the tools you use during development :rocket: 

You probably have a:
* Test runner (Jest, Mocha, Ava, etc.)
* Bundler (Parcel, Weback, Rollup, etc.)
* Linter (ESLint, JSHint, etc.)
* Code Formaters (Prettier, js-beautify, etc.)
* Etc (open to suggustions for more tool types)

All of these tools are running their own file watchers, which, besides the oubvious CPU usage concerns, makes it *very* difficult to cordinate these tools. For example, if you wanted to run a bundler, and then in parallel run the code through a code formater, and sequentially run the output of the code formater through the linter all while the test runner is running your tests in the background... good luck :worried:.

You probably just throw a bunch of npm scripts in your package.json, and then tediously open a bunch of terminal windows with different watchers every time you want to start working. Or maybe you just run the bundler on watch, and only invoke the formater, linter and test runner sequentially right before committing, which will be time consuming since your running them sequentially rather than in parrallel.

Regardless, this is all a huge pain, and no matter how/when you like to use your tools, it would be really nice to have something to easily manage this. Symph is completely unopinionated. It doesn't make assumptions about how or when you want to run your tools. All it does, is provide a simple and easy way to declare how you want them to run, and runs them.

One small thing... I didn't actually make Symph yet. It's still in the design stage.


### Experimental custom declaritive config syntax I'm currently playing around with:

**Basic example:**
```
Parcel,
Jest,
{
  Prettify -> ESLint
}  
```
This would be all the config you need to create 3 workers that concurrently run Parcel, Jest, Prettify, and then ESLint once Prettify finishes.


**This syntax can be pretty powerful. For example you can do something like:**
```
Prettify -> {
  ESLint, Jest, Parcel
}
```
This will run Prettify first and then run ESLint, Jest and Parcel in parralel.



**You can even wait for multiple workers:**
```
{
  Prettify -> ESLint,
  Jest
} -> Parcel
```
This will run Jest in the same time as the Prettify+ESLint, and then only run the bundler once unit tests AND format/lint successfully complete.


**Full example of a `symph.config` file:**
```
OnWatch:
  Parcel -> HMR,
  Jest,
  {
    Prettify -> ESLint
  }  
  
BeforeCommit:
  Jest,
  ESLint
```

Note: In the future Symph can have a GUI that shows a diagram of these build proccesses in a diagram generated side-by-side as you edit your config file.
