# Project Setup

The initial boilerplate code for the project is available at [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.1) and to get started with the setup, run the following commands in your terminal:

```
$ npm install
$ bower install
$ pulp build --to index.js
```

The final command compiles our PureScript code and writes it to `index.js`

At this point, if you open the `index.html` file in your browser you will not see any output. This is what we will build up by first starting with getting an initial layout on the screen.



## Getting the Initial layout on the screen

We imagine our screen and it's actions\(events\) as a set of types. What this means is that, we get to control our application and also make our app less error prone with the help of types. This also gives us a bit of assurance that we are restricted by the types we define. To explain this in detail, we will start by defining our types for our screen in `src/Types.purs` 

```
-- | This is our screen which takes a state
data MainScreen = MainScreen MainScreenState

-- | These are the possible states our MainScreen could be in
data MainScreenState = MainScreenInit | MainScreenAbort

-- | Here we list the possible actions from the screen. For now we will just add few dummy actions
data MainScreenAction = MainScreenAction | MainScreenAbortAction
```

If you look at the above code, we have defined here 3 types:

* `MainScreen` : Our screen name
* `MainScreenState` : The state which the current screen can be in
* `MainScreenAction` : The possible actions\(events\) the screen could emit.



