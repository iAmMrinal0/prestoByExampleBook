## Triggering an Action

The code for this section is available on [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.3)

Our goal in this chapter is to trigger an action and log the content from the input box to the console.

So let's begin with our types. We defined a type `MainScreenAction` in the previous section, which currently has dummy actions. We will now modify our action for our goal.

```
data MainScreenAction = MainScreenAddTodo String | MainScreenAbort
```



