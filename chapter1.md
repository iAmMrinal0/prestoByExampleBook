## Getting the Initial layout on the screen

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.2).

We imagine our screen and its actions\(events\) as a set of types. What this means is that, we get to control our application and also make our app less error prone with the help of types. This also gives us a bit of assurance that we are restricted by the types we define. To explain this in detail, we will start by defining our types for our screen in `src/Types.purs`

```haskell
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

For our types, we need to derive a few instances. What are these instances will be explained in the Advanced section. Hence, for now you could follow the instance definitions from [`src/Types.purs`](https://github.com/iAmMrinal0/prestoByExample/blob/797053c17593878773abc06a2753cdd2fa20ef3c/src/Types.purs) on GitHub.

Now that we have defined our types, we need to trigger the screen so that we could view our initial layout. Presto provides a method called `runUI` which takes a screen name i.e. a type which corresponds to a screen. So we will proceed to use that to trigger the screen. To do that we need to edit `appFlow` in `src/Main.purs` as that's the start of our app which currently does not do anything.

```haskell
appFlow :: Flow Unit
appFlow = do
  _ <- runUI (MainScreen MainScreenInit)
  pure unit
```

Add the respective imports for `runUI` from Presto and for `MainScreen` and `MainScreenInit`

One final step is to modify our handler function in `index.html` so we can match our screen name which is `MainScreen`

```
function handleScreenTag(state) {
  switch(state.screen) {
    case "MainScreen":
      switch(state.contents.tag) {
        case "MainScreenInit":
          init();
          break;
      }
      break;
  }
}
```

Run `pulp build --to index.js` and open `index.html` in your browser to view our initial layout.

