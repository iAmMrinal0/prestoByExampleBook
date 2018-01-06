## More API Calls

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.8)

In this section we will make more API calls, to delete todo items and fetch todo items on initial app load. Update the `server.js` file as from the GitHub repository.

We will start by adding our request and response types for deleting todo items

```haskell
newtype DeleteTodoReq = DeleteTodoReq
  { id :: Number
  }

newtype DeleteTodoRes = DeleteTodoRes
  { code :: Int
  , status :: String
  , response :: String
  }

instance makeDeleteTodoReq :: RestEndpoint DeleteTodoReq DeleteTodoRes where
  makeRequest reqBody headers = defaultMakeRequest POST "http://localhost:3000/delete" headers reqBody
  decodeResponse body = defaultDecodeResponse body
```

Make sure to add encode and decode instances as required for the request and response types. And now to modify our screen state and action types which can now support delete state and delete action.

```haskell
data MainScreenState
  = MainScreenInit
  | MainScreenAbort
  | MainScreenAddToList TodoItem
  | MainScreenDeleteFromList Number

data MainScreenAction
  = MainScreenAddTodo String
  | MainScreenAbortAction
  | MainScreenDeleteTodo Number
```

We have modified our actions and so we now need to handle our new action in `handleMainScreenAction` in `src/Main.purs`

```haskell
handleMainScreenAction :: MainScreenAction -> Flow Unit
handleMainScreenAction action =
  case action of
    MainScreenAddTodo todoItem -> addTodoFlow todoItem
    MainScreenDeleteTodo todoId -> deleteTodoFlow todoId
    _ -> pure unit

deleteTodoFlow :: Number -> Flow Unit
deleteTodoFlow todoId = do
  response <- callAPI (Headers [Header "Content-Type" "application/json"]) (API.DeleteTodoReq {id: todoId})
  case response of
    Left err -> pure unit
    Right (API.DeleteTodoRes {response: todoObj}) -> do
      action <- runUI (MainScreen (MainScreenDeleteFromList todoId))
      handleMainScreenAction action
```

We also added a new flow called `deleteTodoFlow` which handles the deletion of the todo item. This calls the API with the ID of the todo item to delete and runs the `MainScreen` with a new state with the id so it could be removed from the screen.

Time to add our other feature to fetch todo items on initial app load. So we need to update our state type for `MainScreenInit` which will now accept `Array TodoItem` as argument. Time to modify our `appFlow`

```haskell
appFlow :: Flow Unit
appFlow = do
  response <- callAPI (Headers [Header "Content-Type" "application/json"]) API.TodoReq
  case response of
    Left err -> pure unit
    Right (API.TodoRes {response: todoItems}) -> do
      action <- runUI (MainScreen (MainScreenInit todoItems))
      handleMainScreenAction action
```

Now we update our `index.html` to handle the data to delete the todo from the screen and also update our screen on initial app load to show our todo items. First we modify our `handleScreenTag` function where we modify `init` and add a new state handler.

```js
function handleScreenTag(state) {
  switch(state.screen) {
    case "MainScreen":
      switch(state.contents.tag) {
        case "MainScreenInit":
          init(state.contents.contents); // init takes todoItems
          break;
        case "MainScreenAddToList":
          addToList(state.contents.contents);
          break;
        case "MainScreenDeleteFromList": // case for the new delete state
          deleteHandler(state.contents.contents);
          break;
      }
      break;
  }
}

function init(todoItems) { // notice that init takes todoItems now
  // rest of existing code
  todoItems.forEach(todo => addToList(todo))
}

function deleteHandler(value) {
  var divList = document.getElementById("todoValues")
  var divToRemove = document.getElementById(value)
  var divToRemove = document.getElementById("todoId"+value) // updated this 
  divList.removeChild(divToRemove)
}

function deleteTodoHandler(id) { // the handler to invoke when delete button is clicked
  var event = {tag: "MainScreenDeleteTodo", contents: id}
  window.callBack(JSON.stringify(event))()
}

// update the next line of code in the addToList function
button.onclick = () => deleteTodoHandler(todoItem.id)
```

Run the server and build the PureScript code and open `index.html` and try to add, delete todo items and reload the page to see the items loaded on page load.

That's it for the app. If you wish to look at how update todo is implemented, you can check it out on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.9).

