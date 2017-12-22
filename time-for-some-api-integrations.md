## Time for some API integrations

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.6).

So the current state our Todo app is in, we cannot delete individual todo items because they are not associated by any unique ID's. Hence in this section we are going to have a mock server which just sends us an ID which we can use to tag our todo items so we can delete them later when necessary.

To begin with, you could copy the `package.json` and `server.js` files as provided in the repository. Once you do that you can run the command `npm install` to install required node packages.

