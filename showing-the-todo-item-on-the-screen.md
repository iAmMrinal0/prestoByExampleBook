## Showing the Todo item on the screen

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.4)

In the previous section we triggered an action and passed data from our screen to our app logic which we will call Flow from now. In this section we are going to display the content on the screen i.e. pass the data we received from the action and send it back to the screen.

And just like always, our first step is to figure types. To load our initial screen our state was of the following type

```haskell
data MainScreenState = MainScreenInit | MainScreenAbort
```

Now we are going to pass a different state to our screen so we need to extend our `MainScreenState` type

```haskell
data MainScreenState = MainScreenInit | MainScreenAbort | MainScreenAddToList String
```

We have extended our state type to accept a new state with a `String` argument.

It's time to fix our `addTodoFlow` to pass our data back to the screen with the new state.

```haskell
addTodoFlow todoItem = do
  _ <- runUI (MainScreen (MainScreenAddToList todoItem))
  pure unit
```

Here we are using `runUI` again to run the `MainScreen` but now with a different state which is `MainScreenAddToList` with the data we received from our `MainScreenTodoAction`

Now all there's left is to handle the state in our screen in `index.html`

```js
function handleScreenTag(state) {
  switch(state.screen) {
    case "MainScreen":
      switch(state.contents.tag) {
        case "MainScreenInit":
          init();
          break;
        case "MainScreenAddToList":
          addToList(state.contents.contents);
          break;
      }
      break;
  }
}
```

We add a new case to handle `MainScreenAddToList` as that's the name of our state and we add a new `div` in our initial layout so that we can add our items to it. Also we add the function `addToList` which adds our content to the DOM.

```js
function init() {
  document.body.innerHTML = `    <div id="todoInput">
    <input type="text" id="todoItem"/>
    <button onclick="addTodo()">Add Todo</button>
  </div>
  <div id="todoValues">
  </div>`
}
function addToList(todoItem) {
  var p = document.createElement("p")
  p.innerHTML = todoItem
  var div = document.getElementById("todoValues")
  div.appendChild(p)
}
```

As always, build our app using `pulp build --to index.js`. Time to open our `index.html` and view our app in action.

