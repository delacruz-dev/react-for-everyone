# What is React.JS?

React.JS is a JavaScript library which allows you to express any user interface as a declarative function of some parameters, modelled as a `props` object and an internal `state`.

![React allows you to express a user interface as a declarative function of props and state](/assets/react-declarative-function.png)

## Create an HTML element using vanilla JavaScript

Create a new HTML file and add a script to create a new `<div>` element using vanilla JavaScript.

```html
<div id="root"></div>

<script type="text/javascript">
    var root = document.getElementById("root");
    var element = document.createElement("div");
    element.textContent = "Vanilla JS Example";
    element.className = "container";
    root.appendChild(element);
</script>
```

If we inspect `element` with the console, we will realize that it's just the HTML we expected to create.

## Create a React element using React.createElement

It would be handy to encapsulate this behaviour to create HTML elements in a function, so we can reuse it. This is what React gives us: a function to create HTML elements. This function is `createElement` and receives three parameters:

-   The first one is the type of HTML tag we want to use
-   The second one are the attributes the element is going to have
-   The third one are the children of the HTML tag

Let's see the equivalent React version of the above script:

```html
<div id="root"></div>

<!-- In order to use React, add these two scripts to your page -->
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>

<script type="text/javascript">
    var root = document.getElementById("root");
    var element = React.createElement("div", { className: "container" }, "React Example");
    ReactDOM.render(element, root);
</script>
```

If we inspect `element` now with the console, we will realize that it's not an HTML element anymore. Instead, it's a JSON representation of that HTML. This is what we call **Virtual DOM** element.

### A detail about children.

If we add a fourth parameter, `"A second children"`, and then inspect with the console the `element` again, we will realize that it has become an array now.

```javascript
var element = React.createElement(
    "div",
    {
        className: "container"
    },
    "React Example",
    "A second children"
);
```

We can add any number of children as the last parameters, or add it as a property in the second parameter.

```javascript
var element = React.createElement("div", {
    className: "container",
    children: [, "React Example", "A second children"]
});
```

## Create a React element using JSX

Even though creating a whole app using `React.createElement` is completely possible, React team created [JSX](https://facebook.github.io/jsx/), a special JavaScript syntax that looks and works very similar to HTML and behaves in similar ways, so we can write the above code in a way that's a little bit more familiar to us.

```jsx
var root = document.getElementById("root");
var element = <div className="container">JSX Example</div>;
ReactDOM.render(element, root);
```

That will raise a syntax error, given this is not valid JavaScript syntax. We would normally need a build system like [Webpack](https://webpack.js.org/), making use of [Babel](https://babeljs.io/) to transpile this code and transform it into valid JavaScript. But we will just use Babel standalone so we can use it right inside the browser.

Add the following script to your HTML page and change the type of the `<script>` tag from `"text/javascript"` to `"text/babel"`:

```html
<script crossorigin src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
<script type="text/babel">
    {
        /* ... */
    }
</script>
```

### But, is it HTML in JavaScript?

No, JSX is not HTML in JavaScript. It is a special syntax on top of JavaScript that ends up transforming into JavaScript functions after a transpilation process.

JSX has two main differences with HTML:

-   In HTML you use 'class' and in JSX you use 'className'.
-   You can interpolate variables and function within your JSX code by using curly braces:

```jsx
var containerClass = "container";
function sum(a, b) {
    return a + b;
}
var element = <div className={containerClass}>JSX Example {sum(3, 4)}</div>;
```

If we inspect `element` again with the console, we will realize that it's exactly the same as in the example without using JSX. This is because, as we said, JSX is just a special syntax on top of JavaScript. And the work of Babel is to transform this syntax into valid JavaScript.

You can see the output of the transpiled version of the above snippet in [Babel Repl](https://babeljs.io/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=G4QwTgBAxg9gdgFxASzgUzAYQDYgM54QC8EARLIiumKQNwBQAZgK5xQLLwR7MC2AFCAA0EAEYBKCAG96EORDBoEzMHAggIAajEMAvvVCQ02NLzSJiEADwATZMGi4CAORBmiUiklQYc-PLoAfABSAMoAGhAAogAebgAOJtI8AgDMIgAs4rpWAPR2wIG0QA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=6.26.0&envVersion=)

## Parameterize a JSX Element with props

```html
<script type="text/babel">
    var root = document.getElementById("root");

    var element = function(props) {
        return <div className={props.className}>{props.children}</div>;
    };

    ReactDOM.render(<element className="container">Props Example</element>, root);
</script>
```
