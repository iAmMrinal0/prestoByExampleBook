# Presto by Example

[Presto](https://github.com/juspay/purescript-presto) is a framework written in PureScript which simplifies the application development process with the help of simple abstractions. This book is a follow along guide for writing a basic Todo application using Presto.

## Prerequisites

1. JavaScript
2. A bit of PureScript

# Project Setup

The initial boilerplate code for the project is available at [GitHub](https://github.com/iAmMrinal0/prestoByExample/releases/tag/v0.1) and to get started with the setup. We need the following before we can start our setup:

* Node
* Git

Run the following commands in your terminal:

```
$ npm install -g purescript pulp bower
$ npm install
$ bower install
$ pulp build --to index.js
```

The final command compiles our PureScript code and writes it to `index.js`

At this point, if you open the `index.html` file in your browser you will not see any output. This is what we will build up by first starting with getting an initial layout on the screen.

