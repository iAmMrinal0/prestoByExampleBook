## Fixing our Flow

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.5).

If you have been following along you might have noticed that we can't seem to add items after the first time. The reason for this is that once we encounter the `MainScreenAddTodo` action we are going to our next Flow which is `addTodoFlow` where we are not listening\(handling\) any actions from our screen. This is the code we have currently:

```haskell
addTodoFlow todoItem = do
  _ <- runUI (MainScreen (MainScreenAddToList todoItem))
  pure unit
```

Notice that `_` ? We are discarding our action when we should have handled it so we could add more todo items. Let's fix this by writing a method to handle our actions. And now our `appFlow` and `addTodo` flow look like this:

```haskell
appFlow :: Flow Unit
appFlow = do
  action <- runUI (MainScreen MainScreenInit)
  handleMainScreenAction action

handleMainScreenAction :: MainScreenAction -> Flow Unit
handleMainScreenAction action =
  case action of
    MainScreenAddTodo todoItem -> addTodoFlow todoItem
    _ -> pure unit

addTodoFlow :: String -> Flow Unit
addTodoFlow todoItem = do
  action <- runUI (MainScreen (MainScreenAddToList todoItem))
  handleMainScreenAction action
```

So, any action we encounter is passed along to `handleMainScreenAction` where we handle the respective actions; currently only `MainScreenAddTodo`

Time to build our app and open our `index.html` and you will find that we have fixed the issue.

