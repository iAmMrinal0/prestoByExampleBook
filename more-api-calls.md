## More API Calls

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.8)

In this section we will make more API calls, to delete todo items and fetch todo items on initial app load. Update the `server.js` file as from the GitHub repository.

We will start by adding our request and response types for deleting todo items

```
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

Make sure to add encode and decode instances as required for the request and response types.

