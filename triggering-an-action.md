## Triggering an Action

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.3).

Our goal in this chapter is to trigger an action and log the content from the input box to the console.

So let's begin with our types. We defined a type `MainScreenAction` in the previous section, which currently has dummy actions. We will now modify our action for our goal.

```haskell
data MainScreenAction = MainScreenAddTodo String | MainScreenAbortAction
```

We renamed `MainScreenAction` to `MainScreenAddTodo` which now has an argument of type `String` That's it for our types. Time to handle our action, so open up `src/Main.purs` and notice the following code block:

```haskell
appFlow :: Flow Unit
appFlow = do
  _ <- runUI (MainScreen MainScreenInit)
  pure unit
```

Currently, we are discarding the action received by using `_ <- runUI (MainScreen MainScreenInit)` which we will now handle.

```haskell
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

```js
exports.logAny = function(data) {
  console.log(data)
}
```

Now we add a type definition for our foreign function in `src/Runner.purs`

```
foreign import logAny :: forall a. a -> Unit
```

We have our PureScript code ready. Now we need to modify our `index.html` to trigger an action from our button

```js
function init() {
  document.body.innerHTML = `    <div id="todoInput">
    <input type="text" id="todoItem"/>
    <button>Add Todo</button>
    <button onclick="addTodo()">Add Todo</button>
    </div>`
   }
function addTodo() {
  var inputData = document.getElementById('todoItem').value
  var event = {tag: "MainScreenAddTodo", contents:inputData}
  window.callBack(JSON.stringify(event))()
}
```

Notice that our action name\(tag\) is same as the type we defined `MainScreenAddTodo` and the type `String` is the value which we send in `contents`

And we are done with the code. Let's build the PureScript code using `pulp build --to index.js` and open `index.html`

Have the console of your browser open on the side and enter something in the input box and click the button. You should see the result in your console.

