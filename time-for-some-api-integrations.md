## Time for some API integrations

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.6).

So the current state our Todo app is in, we cannot delete individual todo items because they are not associated by any unique ID's. Hence in this section we are going to have a mock server which just sends us an ID which we can use to tag our todo items so we can delete them later when necessary.

To begin with, you could copy the `package.json` and `server.js` files as provided in the repository. Once you do that you can run the following command to install required node packages. After that start our server using the next command.

```
$ npm install
$ npm run server
```

Now that our server is running, we get back to our PureScript code and can you guess what we are going to start with? If you are not sure, then the answer is types. We start by defining our request and response types which helps us in restricting possibilities of errors. The following is our `src/Remote.purs` where we define our request and response types.

```haskell
module Remote where

import Data.Foreign.Class (class Decode, class Encode)
import Data.Generic.Rep (class Generic)
import Presto.Core.Types.API (class RestEndpoint, Method(GET), defaultDecodeResponse, defaultMakeRequest_)
import Presto.Core.Utils.Encoding (defaultDecode, defaultEncode)

data TodoReq = TodoReq
newtype TodoRes = TodoRes
  { code :: Int
  , status :: String
  , response :: String
  }

instance makeTodoReq :: RestEndpoint TodoReq TodoRes where
  makeRequest _ headers = defaultMakeRequest_ GET "http://localhost:3000" headers
  decodeResponse body = defaultDecodeResponse body

derive instance genericTodoReq :: Generic TodoReq _
instance encodeTodoReq :: Encode TodoReq where encode = defaultEncode

derive instance genericTodoRes :: Generic TodoRes _
instance decodeTodoRes :: Decode TodoRes where decode = defaultDecode
```

We will not be going into details about the instances. The advanced section will explain that but we will try to make sense of the code above.

* `TodoReq` : request type
* `TodoRes` : response type \(our actual response will be in the `response` key\)
* `defaultMakeRequest_` : Method to use for `GET` requests
* `GET` : our HTTP request method
* `http://localhost:3000` : our request URL

Before we proceed further, we will have to fix our existing type for `MainScreenAddToList` because now with the todo item we will be passing our unique ID too. So let's fix it.

```haskell
data MainScreenState
  = MainScreenInit
  | MainScreenAbort
  | MainScreenAddToList String String
```

Let's modify our `addTodoFlow` to make the API call and send the response with the unique ID to the screen.

```
addTodoFlow :: String -> Flow Unit
addTodoFlow todoItem = do
  response <- callAPI (Headers []) API.TodoReq
  case response of
    Left err -> pure unit
    Right (API.TodoRes {response: id}) -> do
      action <- runUI (MainScreen (MainScreenAddToList todoItem id))
      handleMainScreenAction action
```

Here we are using a method called `callAPI` which takes the required headers and the request body. As we don't have any headers to pass we just pass `Headers []` and similarly as it's a `GET` request we don't have a request body so we use our request type i.e. `TodoReq` . Once we get the response there's a possibility that our API call failed for some reason and that's what we do a `case` match on. Right now we will concentrate on our successful response that is in the `Right` .

Now that we have our successful response we destructure it and extract the `response` key which holds our response and pass it to the screen with our existing screen state `MainScreenAddToList`

Add the required imports from [`src/Main.purs`](https://github.com/iAmMrinal0/prestoByExample/blob/2b45b00eff8662c9b1763e044113ad071ce7a94c/src/Main.purs) And now time to handle the our id on our screen. So modify `addToList` function in `index.html`

```
function deleteHandler(value) {
  var divList = document.getElementById("todoValues")
  var divToRemove = document.getElementById(value)
  divList.removeChild(divToRemove)
}
function addToList(todoItem) {
  var div = document.createElement("div")
  div.id = "todoId" + todoItem[1]
  var p = document.createElement("p")
  p.innerHTML = todoItem[0]
  var button = document.createElement("button")
  button.innerHTML = "Delete"
  button.onclick = () => deleteHandler(div.id)
  div.appendChild(p)
  div.appendChild(button)
  var divList = document.getElementById("todoValues")
  divList.appendChild(div)
}
```

Build our app in a new terminal, also make sure the server is running and then open `index.html`

Now we have unique ID's for our todo items. In the next section we will change our logic to save our todo items on the server.

