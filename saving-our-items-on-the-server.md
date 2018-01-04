## Saving our items on the server

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.7).

So far we have been just managing our todo items in the DOM and we lose them when we reload our app. In this section we will add new types to add a todo item and call the new API to store it on the server.

Update your `server.js` from the repository. And now let's begin with adding new request and response types for this.

```haskell
newtype TodoRes = TodoRes
  { code :: Int
  , status :: String
  , response :: Array TodoItem
  }

newtype TodoItem = TodoItem
  { id :: Number
  , value :: String
  }

newtype AddTodoReq = AddTodoReq
  { todoItem :: String
  }

newtype AddTodoRes = AddTodoRes
  { code :: Int
  , status :: String
  , response :: TodoItem
  }
```

If you notice we have modified our existing `TodoRes` type and added a type called `TodoItem` with out required request and response types which are `AddTodoReq` and `AddTodoRes` respectively. Our response will be a JSON with `id` and `value` as keys. Now we write our instance as follows:

```haskell
instance makeAddTodoReq :: RestEndpoint AddTodoReq AddTodoRes where
  makeRequest reqBody headers = defaultMakeRequest POST "http://localhost:3000/add" headers reqBody
  decodeResponse body = defaultDecodeResponse body
```

* `AddTodoReq`: request type
* `AddTodoRes`: response type \(our actual response will be in the `response` key\)
* `defaultMakeRequest`: Method to use for `POST` requests
* `POST`: our HTTP request method
* `http://localhost:3000/add`: our request URL

Add the required `encode` and `decode` instances for the request and response types. Now we update our `addTodoFlow` to make the API call with our updated request type and handle the appropriate response type.

```haskell
addTodoFlow :: String -> Flow Unit
addTodoFlow todoItem = do
  response <- callAPI (Headers [Header "Content-Type" "application/json"]) (API.AddTodoReq {todoItem: todoItem})
  case response of
    Left err -> pure unit
    Right (API.AddTodoRes {response: todoObj}) -> do
      action <- runUI (MainScreen (MainScreenAddToList todoObj))
      handleMainScreenAction action
```

Now we need to fix our screen types because we are now passing a todo object to the screen but our `MainScreenAddToList` takes 2 `String` as arguments. So we update the type to now accept `TodoItem`

```haskell
data MainScreenState
  = MainScreenInit
  | MainScreenAbort
  | MainScreenAddToList TodoItem
```

Time to update our frontend code to update the todo item based on the new type i.e. todo object. We have only modified line 39 and 41 in `index.html`

```js
function addToList(todoItem) {
  var div = document.createElement("div")
  div.id = "todoId" + todoItem.id
  var p = document.createElement("p")
  p.innerHTML = todoItem.value
  var button = document.createElement("button")
  button.innerHTML = "Delete"
  button.onclick = () => deleteHandler(div.id)
  div.appendChild(p)
  div.appendChild(button)
  var divList = document.getElementById("todoValues")
  divList.appendChild(div)
}
```

Time to run our server and build our PureScript code and open `index.html` to see our changes in action.

