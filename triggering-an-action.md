## Triggering an Action

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.3)

Our goal in this chapter is to trigger an action and log the content from the input box to the console.

So let's begin with our types. We defined a type `MainScreenAction` in the previous section, which currently has dummy actions. We will now modify our action for our goal.

```
data MainScreenAction = MainScreenAddTodo String | MainScreenAbortAction
```

We renamed `MainScreenAction` to `MainScreenAddTodo` which now has an argument of type `String` That's it for our types. Time to handle our action, so open up `src/Main.purs` and notice the following code block:

```
appFlow :: Flow Unit
appFlow = do
  _ <- runUI (MainScreen MainScreenInit)
  pure unit
```

Currently, we are discarding the action received by using `_ <- runUI (MainScreen MainScreenInit)` which we will now handle.

```
appFlow :: Flow Unit
appFlow = do
  action <- runUI (MainScreen MainScreenInit)
  case action of
    MainScreenAddTodo todoItem -> addTodoFlow todoItem
    _ -> pure unit

addTodoFlow :: String -> Flow Unit
addTodoFlow todoItem = pure (logAny todoItem)
```

We are using a method `logAny` which we have not defined yet. Let's define it as a foreign function. To read more about Foreign Function Interface\(FFI\), you could follow this [document](https://github.com/purescript/documentation/blob/master/language/FFI.md).

In our `src/Runner.js` we define our `logAny` which is just a function that does `console.log`

```
exports.logAny = function(data) {
  console.log(data)
}
```



