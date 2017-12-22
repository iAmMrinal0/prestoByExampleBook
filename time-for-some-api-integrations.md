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

```
data MainScreenState
  = MainScreenInit
  | MainScreenAbort
  | MainScreenAddToList String String
```



