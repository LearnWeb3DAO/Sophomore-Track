# Just Enough React (and Next.js)

In the Freshman Track, we built a very simple dApp that used HTML, CSS, and some Javascript. In the real world, however, these type of 'vanilla' website implementations are a thing of the past.

Today, we use web frameworks to simplify the web development process. Is it simpler though? Depends on whose perspective you're looking from. If you're just starting out and have never done this before, it may take some time till you understand all the necessary concepts. But, in the long run, you will thank yourself and be happy that you took the time to learn it.

The biggest and most common web frameworks used today are:

- [React](https://reactjs.org)
- [Angular](https://angular.io)
- [Vue](https://vuejs.org)

While they each have their own pros and cons, React has by far taken the web development world by storm. The same is true in the Web3 space, where React is the most commonly used web framework for building dApps. Throughout the Sophomore track, and all later tracks as well, we will be using a lot of React, so think of this level as a React crash course which will teach you _just enough_ to get started. This is not meant to be a replacement for learning React on platforms that focus on teaching Web2, but is to be used as a guide to understand just how much is necessary to get started so you don't get stuck in tutorial hell. 😅

> Actually, we will be using Next.js - which is an extension of React itself - but more on that later. Let's learn some React first.

## WTF is React?

React is a web framework that makes it easy to build and reason about the 'view' of your web app. The 'view' is what is displayed on the screen, how it changes, how it updates, etc. React essentially just gives you a template language, and you can **create Javascript functions that return HTML.**

Normal Javascript functions return Javascript-esque things - strings, numbers, booleans, objects, etc. React basically combined Javascript and HTML to produce a language they call **JSX**. In JSX, Javascript-like functions return HTML instead of regular Javascript things. That's basically it.

<Quiz questionId="8c7c3dd2-2297-4cdb-933b-93c38fb64ecd" />

The combination of Javascript functions that return HTML are called **Components**. Components are written in JSX. Though seemingly awkward at first, they are actually quite easy to work with once you get used to it.

Here's an example of a simple component:

```jsx
function ShoppingList() {
  return (
    <div className="shopping-list">
      <h1>Shopping List</h1>
      <ul>
        <li>Apples</li>
        <li>Bananas</li>
        <li>Grapes</li>
      </ul>
    </div>
  );
}
```

<CodeSandbox title="icy-water-xrm2ch" />

Great, but this isn't much different from HTML. But what if you wanted to render a list of items based on an array?

```jsx
function ShoppingList() {
  const items = ["Apples", "Bananas", "Grapes"];

  return (
    <div className="shopping-list">
      <h1>Shopping List</h1>
      <ul>
        {items.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
}
```

<CodeSandbox title="confident-tesla-5l3k1t" />

Wow, look at that! We just used Javascript inside HTML. In JSX, you can write Javascript inside HTML by wrapping the JS code in curly braces `{` and `}`. Actually though, if you think about this a little more, you will realize what is happening. `.map()` is a Javascript function, which loops over an array and returns something for each item. In this case, it is looping over the `items` array and returning an `li` element, that has the value of the Javascript variable `item` within it, which is HTML. Get it? 🤔

Inside our component, we basically embedded another component. The map function is a JS function that returns HTML. It is a component. Even though it is not explicitly defined as a top level function, it is still a component.

**Embedding components inside other components is the power of React.** This is called **Composition**. We are composing together multiple Javascript functions that return HTML, and building up a single combined HTML document from it that will be displayed on the web app.

<Quiz questionId="4ac6b0df-759d-4f46-a67b-175c55d75aa6" />

## Data Passing between Components

Components are not very useful if they are just static. Sure, looping over arrays and things are fine, but most web apps today are not static documents. Most web apps today will dynamically fetch data from some sort of server, database, or blockchain. This means the same component will often times be required to display different data.

The main use case of having components is being able to write code that is reusable and can, within it, contain different information without rewriting the entire code again.

Let's take a look at an example. Which of these two codes is more readable?

```html
<div class="cards">
  <div class="card">
    <img src="img_avatar.png" alt="Avatar" />
    <div class="container">
      <h4><b>Alice</b></h4>
      <p>Frontend Developer</p>
    </div>
  </div>

  <div class="card">
    <img src="img_avatar.png" alt="Avatar" />
    <div class="container">
      <h4><b>Bob</b></h4>
      <p>Backend Developer</p>
    </div>
  </div>

  <div class="card">
    <img src="img_avatar.png" alt="Avatar" />
    <div class="container">
      <h4><b>Charlie</b></h4>
      <p>Full Stack Developer</p>
    </div>
  </div>
</div>
```

```jsx
function Cards() {
    return (
        <div className="cards">
            <!--Data is passed to children through HTML attributes-->
            <Card name="Alice" job="Frontend Developer" />
            <Card name="Bob" job="Backend Developer" />
            <Card name="Charlie" job="Full Stack Developer" />
        </div>
    )
}

// Card receives an object as an argument
// We can destructure the object to get specific variables
// from inside the object - name and job
function Card({name, job}) {
    return (
        <div className="card">
            <img src="img_avatar.png" />
            <div className="container">
                <h4><b>{name}</b></h4>
                <p>{job}</p>
            </div>
        </div>
    )
}
```

<CodeSandbox title="zealous-carson-44om2s" />

The plain HTML example reused the same code three times, even though the only thing that actually changed was just the name of the person and their job title.

In JSX, we can abstract away each `Card` as a component, which takes certain data from it's parent component (in this case, `Cards`). parent component passes data to the child through HTML-like attributes (`name="Alice"`), and the child accesses their values like a JS function receiving a JS object as an argument. The `Card` component can then return HTML with variable data inside it depending on what it received from the parent.

<Quiz questionId="febb9535-244f-4836-abd2-8aa06351f8a2" />

This code is much more easily reusable and extendable. Want to slightly change how all the cards look? Just modify a single component! Not all the copy-pasted HTML.

<Quiz questionId="e7dfcdde-d2e1-47ec-947a-1a134ce8378f" />

## Interactive Components

Alright, so we can now pass data between components. That is all good, but we haven't yet added interactivity. Things like being able to run some code when a button is clicked, or when text is typed into an input box, etc.

Thankfully, in Javascript, functions can contain functions within them. For example,

```js
function someFunc() {
  function otherFunc() {
    console.log("Hello!");
  }

  otherFunc();
}

someFunc(); // will print "Hello!"

otherFunc(); // will throw an error! undefined here
```

`otherFunc` is only available for use inside the `someFunc` itself. This is a rarely used feature in regular Javascript, but is very heavily used when working with React. Let's see why with an example.

<Quiz questionId="bda73acd-8b42-4918-a2d8-e3a1b5596f3e" />

```jsx
function Button() {
  function handleClick() {
    console.log("Hello");
  }

  return (
    <button className="button" onClick={handleClick}>
      Click Me!
    </button>
  );
}
```

<CodeSandbox title="quizzical-sammet-59fi5i" />

We have a JSX Function called `Button`. Within this function, we have another function called `handleClick`. In the HTML `<button>` tag, we specify `onClick={handleClick}` so when the button is clicked, the `handleClick` function is called. This function is only available inside the `Button` component. Clicking the button on the web app will print `Hello` in the browser console. This is how we build interactive websites with React!

That example is still fairly simple, because `handleClick` takes no arguments. What if we want to print text in the console as the user is typing into an input box? How do we pass the text to the function?

Here's how.

```jsx
function PrintTypedText() {
  function handleOnChange(text) {
    console.log(text);
  }

  return <input type="text" onChange={(e) => handleOnChange(e.target.value)} />;
}
```

<CodeSandbox title="mutable-feather-el4sbe" />

The HTML `input` element provides a handy event listener - `onChange` - that is triggered every time the text in that input box changes (typing a new character, deleting a character, etc.).

Along with triggering a function though, it also passes along the HTML element that changed (referred to as `e` here). We can then take the HTML element `e` and extract the text using `e.target.value` and pass it as an argument to `handleOnChange` which will log the text to the browser console.

Different HTML elements have different event handlers - `onChange` and `onClick` are two examples, but there are many more! You can find a list of all HTML events [here](https://www.w3schools.com/jsref/dom_obj_event.asp).

By combining HTML events with function handlers, we can do all sorts of cool things! Load data from a server, send data to a server, update our view, etc.

## React Hooks - useState and useEffect

Alright, we have gone over Composition, Data Passing, and Interactivity. But, our apps are still quite dumb. Interactivity will allow you to run some code on button clicks etc. but what if you want to update some variables?

Unfortunately, the following **does not** work

```jsx
function DoesNotWork() {
  let myNumber = 0;

  function increment() {
    myNumber++;
  }

  return (
    <div>
      <p>{myNumber}</p>
      <button onClick={increment}>Increment!</button>
    </div>
  );
}
```

<CodeSandbox title="romantic-euclid-44hh8x" />

No matter how many times you click the `Increment` button, the displayed number on the screen will be stuck at `0`. This is because when you update regular variables like `myNumber` from within a React component, even though the value is updated, React does not actually re-render the view of the web app. It does not automatically update the HTML view of the page.

<Quiz questionId="4665f339-24c6-41ea-8e93-9cf8de501f5b" />

React Hooks are functions that 'hook' into different parts of React Components allowing you to do things like updating the view when a variable's value is changed, or running some JS code automatically every time the page is loaded or a variable is changed, and many more cool things! We will primarily focus on three React hooks which are used 95% of the time - **`useState`, `useEffect`, and `useRef`**.

### useState

There are a lot of times when you want the HTML view to update based on some variable's value changing. We can use the `useState` hook to maintain a variable which automatically re-renders the HTML displayed on the screen every time its value is changed. Here's an example:

```jsx
function ThisWorks() {
  // myNumber is the variable itself
  // setMyNumber is a function that lets us update the value
  // useState(0) initializes the React Hook
  // with the starting value of 0
  const [myNumber, setMyNumber] = useState(0);

  function increment() {
    // Sets the new value to the old value + 1
    setMyNumber(myNumber + 1);
  }

  return (
    <div>
      <p>{myNumber}</p>
      <button onClick={increment}>Increment!</button>
    </div>
  );
}
```

<CodeSandbox title="intelligent-hoover-31bwmg" />

If you try to run the above code, you will see the view of the web app automatically gets updated to reflect the new value of the variable.

Variables created using `useState` in React are called **State Variables**. State Variables can be updated and automatically update the view of the app. Here's another example of using State Variables with input boxes.

```jsx
function StateWithInput() {
  // myName is the variable
  // setMyName is the updater function
  // Create a state variable with initial value
  // being an empty string ""
  const [myName, setMyName] = useState("");

  function handleOnChange(text) {
    setMyName(text);
  }

  return (
    <div>
      <input type="text" onChange={(e) => handleOnChange(e.target.value)} />
      <p>Hello, {myName}!</p>
    </div>
  );
}
```

<CodeSandbox title="young-microservice-c1e5be" />

We see the text displayed on the HTML changes as the content of the input box changes. Great!

<Quiz questionId="27b8acaf-2d9b-4b55-b88f-fd521e3c1aac" />

Last thing I want to say about useState is that you can also use it for more than just basic types like strings and numbers. You can also use them to store arrays and objects. However, there is a caveat here. Let's look at an example:

```jsx
function StateArrayDoesNotWork() {
  const [fruits, setFruits] = useState([]);
  const [currentFruit, setCurrentFruit] = useState("");

  function updateCurrentFruit(text) {
    setCurrentFruit(text);
  }

  function addFruitToArray() {
    fruits.push(currentFruit);
  }

  return (
    <div>
      <input type="text" onChange={(e) => updateCurrentFruit(e.target.value)} />
      <button onClick={addFruitToArray}>Add Fruit</button>

      <ul>
        {fruits.map((fruit, index) => (
          <li key={index}>{fruit}</li>
        ))}
      </ul>
    </div>
  );
}
```

<CodeSandbox title="kind-jasper-vbs0f9" />

If you try to run this, you will see no fruits get displayed on the screen. Also notice that we are not using the `setFruits` function anywhere, instead just trying to `.push` to the `fruits` array.

When we try to update the array directly, React does not register the state change, and that is also considered an invalid state update which can cause the application to behave unexpectedly. We know we need to somehow use `setFruits`, but how? The answer is that we actually need to create a copy of the fruits array, add the fruit to it, and set the state variable to be the new array entirely. Example below:

```jsx
function StateArray() {
  const [fruits, setFruits] = useState([]);
  const [currentFruit, setCurrentFruit] = useState("");

  function updateCurrentFruit(text) {
    setCurrentFruit(text);
  }

  function addFruitToArray() {
    // The spread operator `...fruits` adds all elements
    // from the `fruits` array to the `newFruits` array
    // and then we add the `currentFruit` to the array as well
    const newFruits = [...fruits, currentFruit];
    setFruits(newFruits);
  }

  return (
    <div>
      <input type="text" onChange={(e) => updateCurrentFruit(e.target.value)} />
      <button onClick={addFruitToArray}>Add Fruit</button>

      <ul>
        {fruits.map((fruit, index) => (
          <li key={index}>{fruit}</li>
        ))}
      </ul>
    </div>
  );
}
```

<CodeSandbox title="suspicious-monad-gpes3r" />

If you try to run the above code, you will see it works as expected. Every time you press the button, the current text from the input box is added into the array, which causes the list of fruits displayed on the HTML to be updated. You can keep adding as many fruits as you would like!

> The same is true for Objects as well. If your state variable contains an object, you need to create a copy of the object first, update a value, and then set the state variable to the new object entirely.

## useEffect

So we can manage state now, that's great! State changes also affect our rendered HTML, also great!

But, often times, there is a need to automatically run some code when the page is first loaded - perhaps to fetch data from a server or the blockchain - and also the need to automatically run some code when a certain state variable changes.

These type of functions are called side effects. React gives us the `useEffect` hook which allows us to write these kinds of effects. `useEffect` takes two arguments - a function and a dependency array. The function is the code that runs when the effect is run, and the dependency array specifies _when_ to trigger the side effect.

Consider an example where when the website is first loaded, it wants to load some data from the server. While doing so, it wants to display the user a loading screen, and then once the data has been loaded, remove the loading screen and show the actual content. How do we do this?

```jsx
function LoadDataFromServer() {
  // Create a state variable to hold the data returned from the server
  const [data, setData] = useState("");
  // Create a state variable to maintain loading state
  const [loading, setLoading] = useState(false);

  async function loadData() {
    // Set `loading` to `true` until API call returns a response
    setLoading(true);

    // Imaginary function that performs an API call to load
    // data from a server
    const data = await apiCall();
    setData(data);

    // We have the data, set `loading` to `false`
    setLoading(false);
  }

  // loadData is the function that is run
  // An empty dependency array means this code is run
  // once when the page loads
  useEffect(() => {
    loadData();
  }, []);

  // Display `"Loading..."` while `loading` is `true`,
  // otherwise display `data`
  return <div>{loading ? "Loading..." : data}</div>;
}
```

<CodeSandbox title="practical-cache-ib3c0f" />

If you run the above code from the link, you will see it displays `Loading...` for 5 seconds on the screen, and then displays `ABCDEF`. It's because `apiCall` is a function that just waits for 5 seconds and then returns the string `ABCDEF`.

The `useEffect` calls `loadData` when the page is first loaded - thanks to the empty dependency array - and the state variables make the HTML render the appropriate content.

---

This is good for running code when the page first loads, but what about running some piece of code every time a state variable's value changes? For example, when you're searching for a person's name on Facebook, how does Facebook fetch and display recommendations every time you add/remove a character?

You can also do that with `useEffect`, by providing the state variable in the dependency array. Every time the value of that variable changes, the effect will be run.

```jsx
function DependentEffect() {
  const names = ["Alice", "Bob", "Charlie", "David", "Emily"];

  const [recommendations, setRecommendations] = useState([]);
  const [searchText, setSearchText] = useState("");

  useEffect(() => {
    // If user is not searching for anything, don't show any recomendations
    if (searchText.length === 0) {
      setRecommendations([]);
    }
    // Else, find recommendations
    else if (searchText.length > 0) {
      const newRecs = names.filter((name) =>
        name.toLowerCase().includes(searchText.toLowerCase())
      );
      setRecommendations(newRecs);
    }
  }, [searchText]);

  return (
    <div>
      <input type="text" onChange={(e) => setSearchText(e.target.value)} />
      <h2>Recommendations:</h2>
      <ul>
        {recommendations.map((rec, index) => (
          <li key={index}>{rec}</li>
        ))}
      </ul>
    </div>
  );
}
```

<CodeSandbox title="cold-darkness-eub0eh" />

<Quiz questionId="e27e7555-0f31-429f-ac77-207373149378" />

If you run the above code, and try typing some letters, you will see the recommendations list automatically gets updated as you add/remove new characters in the search box. This is because when you update the input box, the value of `searchText` is updated through the `onChange` handler, which triggers the `useEffect`, which updates the `recommendations` list, which updates the HTML view.

<Quiz questionId="53346a89-ee63-4318-b9c4-ea016e0b5def" />

You can also similarly create side effects which are dependent on multiple state variables, not just one. If any of the dependent variables change, the side effect is run. You do this by just adding more state variables to the dependency array.

```jsx
useEffect(() => {
  // Some code
}, [stateVar1, stateVar2, stateVar3, andSoOn]);
```

<Quiz questionId="3f615f81-ab26-486b-9665-e9615f4a1b8d" />

### useRef

`useRef` is another React hook that is somewhat commonly used. It is quite similar to `useState` on the surface, but has some subtle differences that are actually quite important which make this React Hook important to learn.

`useRef` variables are created as follows:

```jsx
function Component() {
  const myValue = useRef();

  function updateMyValue(newValue) {
    myValue.current = newValue;
  }

  function printMyValue() {
    console.log(myValue.current);
  }
}
```

#### #1 - No Re-Rendering

Similar to `useState`, the `useRef` hook also allows us to store a variable in a component that can be updated over time. But, unlike state variables, updating the value of a ref variable does not cause the HTML view to re render.

Therefore, if you had a `useRef` variable and you were displaying it's value in your HTML view, updating the variable will **not** update the HTML view.

```jsx
function CounterWithRef() {
  const myNumber = useRef();

  function increment() {
    if (myNumber.current !== undefined) {
      myNumber.current += 1;
    } else {
      myNumber.current = 1;
    }
    console.log(myNumber.current);
  }

  return (
    <div>
      <p>{myNumber}</p>
      <button onClick={increment}>Increment!</button>
    </div>
  );
}
```

<CodeSandbox title="small-paper-9ecbfp" />

If you run the above code, you will notice that every time you click the button, the value is incremented and printed in the browser console, but the HTML view doesn't actually update. In fact, the HTML view doesn't display anything, because the initial value of `myNumber.current` is `undefined` and since the HTML doesn't update, it stays `undefined` as far as the HTML is concerned even though the value is actually being updated.

#### #2 - Synchronous Updates

![](https://i.imgur.com/QZk8FcK.jpg)

![](https://i.imgur.com/ujZNqIA.jpg)

Something we didn't mention before about `useState` is that when we update a state variable using the `setXYZ` functions, it actually doesn't get updated instantly.

Setting a new value for state variables happens asynchronously in React, which means if you try to use the state variable's value immediately after setting it to a new value, you might not actually see the new value being reflected as it happens asynchronously.

Let's take a look at the Counter example again when using `useState`.

```jsx
function AsyncStateVariables() {
  const [number, setNumber] = useState(0);

  function increment() {
    setNumber(number + 1);
    console.log(number);
  }

  return (
    <div>
      <p>{number}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

<CodeSandbox title="reverent-lichterman-k4n448" />

When you run this, notice what happens on the view and what happens in the console. When you first click the button, the state variable should be updated to `1` - and that's what happens on the view, the web page displays `1`. But if you look at the browser console, the value `0` is printed instead of `1`. This pattern continues as you keep clicking the button.

This is because the `setNumber` call runs asynchronously, and by the time we reach the `console.log(number)` line, the value hasn't been updated yet, so it prints the old value of `number`. When it does in fact gets updated, the HTML is re-rendered to display the new value.

---

`useRef` on the other hand, allows for synchronous updates. When you update the value of a reference variable using `myVar.current = newValue` it is instantly updated, and there is no delay. This can come in handy sometimes.

#### #3 - Referencing DOM Elements

Another cool thing that `useRef` lets us do is that it allows us to refer directly to DOM elements. This is something that is not possible with `useState`.

For example, you can reference an `input` element directly using `useRef`

```jsx
function InputFocus() {
  const inputRef = useRef();

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

<CodeSandbox title="input-focus-zntci" />

When you run this above example, you will notice that as soon as the page loads, the `input` element is already in focus i.e. you can start typing without clicking on it first. This is because we hold a reference to the `input` element, and have a `useEffect` that runs on page load due to having an empty dependency array, that focuses on the `input` element.

<Quiz questionId="a5533750-da4a-42a4-81d0-2afb9009a529" />

## React File Structure

Great, we have covered the main concepts you should know if you're just getting started with React. But so far, we have only dealt with isolated component examples. How does an actual React project look like?

React applications are typically created using a tool like `create-react-app` (CRA). CRA is a command-line tool that helps you setup new React projects and install all the required dependencies without you needing to manually create all the boilerplate.

When you use CRA, you will end up with a file structure that looks like this.

![](https://i.imgur.com/4ANKkGn.png)

The `package.json` file should be recognizable. CRA works through a Node.js environment, and `package.json` is where all the dependencies and project metadata is created - as with any other Node.js project.

The `src/` folder houses the components and CSS styling, basically any React-specific code. The main component here is `App.js`, which is the autogenerated component that is created when you first set up a React application. `index.js` is the main entrypoint of the React application, but typically you would not need to modify it very much (or at all). It just contains some boilerplate React code that takes your components and converts it into actual HTML and JS that can be run in a browser.

The `public/` folder then contains only one file by default - `index.html`. You would usually never touch this yourself. This is a super simple barebones HTML file. When a React app is run, React does some magic under the hood that takes all your components and Javascript code, converts it to actual HTML and JS that can run in the browser, and replaces the contents of `index.html` with all of that. Then, the updated `index.html` is what is displayed to the user.

If you wanted to add images, fonts, music, etc. to your website, they would also go into the `public/` folder. The `public/` folder basically contains everything you want to be directly accessible on your website.

For example, if you added an image called `avatar.png` into your `public/` folder, you can then display that image within a React component as such:

`<img src="/avatar.png" />`

While this may seem weird because your component lives inside the `src/` folder and not the `public/` folder - the reason it works is because the image lives in the same folder as `index.html` - and `index.html` is where your React code actually ends up. So when referencing an image using the relative path of `/avatar.png`, it knows that `avatar.png` must be inside the `public` folder.

## Backend Time

![](https://i.imgur.com/eU81Qd0.png)

So far we have been discussing React, and all it's frontend capabilities. But what about backends?

React is not a backend framework, so if you wanted to create your own API backend, you would have to set up a separate project using something like Node.js and Express. This is however cumbersome, as if the backend and frontend are for the same project, you likely have a lot of code that can be reused and shared across the two. Additionally, maintaining two projects is always harder than maintaining just one.

Enter, Next.js

Next is a meta-framework for React. What does this mean? Well, React itself is a framework for building web apps. Next, on top, is a framework for React that also introduces a few additional features that React did not have.

If you know React, Next is 90% the exact same thing and you can get started using it very quick, but I want to talk about these extra features that Next brings.

First of all, as the title and intro may have hinted to you, Next allows you to write both Frontend and Backend code in a single project. You build your frontend using React, and write your backend API endpoints in a similar syntax as using Express - but all in the same project.

Secondly, Next makes creating multi-page web apps very easy. React was originally designed to help create Single-Page Applications (SPAs), and components are great for that! But what if your website has multiple pages? For example `https://learnweb3.io/` and `https://learnweb3.io/about` and `https://learnweb3.io/tracks` etc.

For this, libraries such as `React Router` were introduced, which made it possible, but also were a bit cumbersome. 'Next' simplified this a lot by allowing for automatic page routing based on filename.

Lastly, Next also features Server Side Rendering (SSR) and Static Site Generation (SSG). These are not features we will use within our tracks, so I will not spend too much time here, but feel free to go through the Recommended Readings later on if you'd like to learn more about them.

### Routing in Next

We will discuss Routing before we discuss creating a backend server, because this will help you understand how that works as well.

Similar to `create-react-app`, Next has a tool called `create-next-app` that can automatically help you setup new Next.js projects with ease.

When you create a new Next.js project, you will end up with a file structure that looks like this:

![](https://i.imgur.com/KDx0RzY.png)

This is lot of files! But don't worry, a lot of it is similar to what we have already discussed for React.

The `public/` folder works exactly the same way, with the exception of not containing an `index.html` file. However, if you wanted to add images, icons, fonts, music, etc. you would place them all in the `public/` folder.

The `styles/` folder is a good addition, providing a dedicated spot for all your CSS files.

The `pages/` is what's great. `_app.js` is an autogenerated file that usually you will not be touching yourself, and sets up some boilerplate code that allows Next to render the proper components.

`pages/index.js` is the homepage of your website. Basically, every file within the `pages` folder is a route of your website. Following Javascript/HTML-style naming conventions, this means that `index` files are the 'main' files. Therefore, `pages/index.js` is the view that will load up when you first open up your website.

If you add more files under the `pages` folder, for example a file named `about.js` - it will be available at `YOUR_DOMAIN/about` (Fun Fact: LearnWeb3's website is created using Next, and that is exactly how our `https://learnweb3.io/about` page works).

This is great because you don't have to deal with things like React Router, and building multi-page websites is as simple as just creating a new file under the `pages/` folder and Next will automatically generate a route based on the filename for you.

You can also do multilevel routing by creating subfolders under the `pages/` folder. For example, something like `pages/tracks/freshman.js` will have the route `YOUR_DOMAIN/tracks/freshman`.

<Quiz questionId="f6735f24-7539-4998-bb46-3e5c00b14afd" />

### Writing APIs in Next

There is one special folder under `pages/` however which was also autogenerated. The `pages/api` folder. Unlike regular pages, which render HTML views, anything under the `pages/api` folder acts as an API endpoint.

Let's look at the autogenerated `pages/api/hello.js` file:

```js
export default function handler(req, res) {
  res.status(200).json({ name: "John Doe" });
}
```

This is a very Express-like function. If you were to go to `YOUR_DOMAIN/api/hello` - instead of rendering an HTML view, this will return a JSON object `{name: 'John Doe'}` - which is a super simple API endpoint.

Similar to regular HTML views, you can create API endpoints under `pages/api` just by creating new files, and the endpoint routes are based on the filename.

<Quiz questionId="ff3c1f4e-f2c4-4d8f-9c51-bec1cc201c01" />

## Conclusion

I hope this article was helpful to you and can act as a crash course. I deliberately focused more on React than Next here, as getting used to the Frontend part is going to be more relevant for us than the Backend part. Also, backend code is basically regular Javascript, whereas Frontend is JSX which I wanted to get you more comfortable with.

I've listed some additional readings and videos that I would recommend to get an even better understanding of these concepts. As always, if you have any questions, message on the `#web2-support` channel on [Discord](https://discord.gg/learnweb3) and we'd be happy to help.

## Readings/Videos

- [Learn React in 30 Minutes](https://www.youtube.com/watch?v=hQAHSlTtcmY)
- [Next.js in 100 Seconds // Plus full tutorial](https://www.youtube.com/watch?v=Sklc_fQBmcs)
- [Scrimba's Full React Course](https://scrimba.com/learn/learnreact)
- [Next.js Crash Course](https://www.youtube.com/watch?v=mTz0GXj8NN0)

## DYOR Questions

<Quiz questionId="0998b476-55c7-4386-9313-e917b15e695a" />

<SubmitQuiz />
