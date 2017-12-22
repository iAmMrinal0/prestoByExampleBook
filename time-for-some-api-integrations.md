## Time for some API integrations

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.6).

So the current state our Todo app is in, we cannot delete individual todo items because they are not associated by any unique ID's. Hence in this section we are going to have a mock server which just sends us an ID which we can use to tag our todo items so we can delete them later when necessary.

To begin with, you could copy the `package.json` and `server.js` files as provided in the repository. Once you do that you can run the following command to install required node packages. After that start our server using the next command.

```
$ npm install
$ npm run server
```

Our server is running, so we get back to our PureScript code and can you guess what we are going to start with? If you are not sure, then the answer is types.

