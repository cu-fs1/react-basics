# React Basics

A comprehensive guide to React fundamentals for students.

## Table of Contents
1. [What is React?](#what-is-react)
2. [JSX](#jsx)
3. [Components](#components)
4. [Props](#props)
5. [State](#state)
6. [Event Handling](#event-handling)
7. [Conditional Rendering](#conditional-rendering)
8. [Lists and Keys](#lists-and-keys)
9. [Forms](#forms)
10. [Component Lifecycle](#component-lifecycle)
11. [Composition vs Inheritance](#composition-vs-inheritance)

---

## What is React?

React is a JavaScript library for building user interfaces. It lets you compose complex UIs from small, isolated pieces of code called "components."

**Key Features:**
- **Declarative**: Design simple views for each state in your application
- **Component-Based**: Build encapsulated components that manage their own state
- **Learn Once, Write Anywhere**: Can develop new features without rewriting existing code

---

## JSX

JSX is a syntax extension to JavaScript that looks similar to HTML. It makes writing React components easier and more intuitive.

### Basic JSX Example

```jsx
const element = <h1>Hello, world!</h1>;
```

### Embedding Expressions in JSX

You can put any valid JavaScript expression inside curly braces in JSX:

```jsx
const name = 'John Doe';
const element = <h1>Hello, {name}!</h1>;

const user = {
  firstName: 'Jane',
  lastName: 'Smith'
};

function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const greeting = <h1>Hello, {formatName(user)}!</h1>;
```

### JSX is an Expression Too

After compilation, JSX expressions become regular JavaScript function calls:

```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### JSX Attributes

You can use quotes to specify string literals as attributes:

```jsx
const element = <img src="profile.jpg" alt="Profile" />;
```

Or use curly braces to embed a JavaScript expression:

```jsx
const element = <img src={user.avatarUrl} alt={user.name} />;
```

**Important:** Since JSX is closer to JavaScript than HTML, React uses camelCase for property naming:
- `className` instead of `class`
- `htmlFor` instead of `for`
- `onClick` instead of `onclick`

---

## Components

Components let you split the UI into independent, reusable pieces.

### Function Components

The simplest way to define a component:

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### Class Components

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Rendering a Component

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```

### Composing Components

Components can refer to other components in their output:

```jsx
function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```

---

## Props

Props (short for "properties") are how you pass data from parent to child components.

### Reading Props

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Usage
<Welcome name="Sara" />
```

### Props are Read-Only

Components must never modify their own props:

```jsx
// ❌ Wrong - Never modify props
function Welcome(props) {
  props.name = 'Modified'; // Don't do this!
  return <h1>Hello, {props.name}</h1>;
}

// ✅ Correct - Props are read-only
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### Passing Multiple Props

```jsx
function UserCard(props) {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>Age: {props.age}</p>
      <p>Email: {props.email}</p>
    </div>
  );
}

// Usage
<UserCard name="John" age={30} email="john@example.com" />
```

### Destructuring Props

```jsx
function UserCard({ name, age, email }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}
```

### Default Props

```jsx
function Button({ text, color }) {
  return <button style={{ backgroundColor: color }}>{text}</button>;
}

Button.defaultProps = {
  text: 'Click me',
  color: 'blue'
};
```

---

## State

State is similar to props, but it is private and fully controlled by the component.

### Using State in Class Components

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      date: new Date(),
      count: 0
    };
  }

  render() {
    return (
      <div>
        <h1>It is {this.state.date.toLocaleTimeString()}</h1>
        <p>Count: {this.state.count}</p>
      </div>
    );
  }
}
```

### Updating State

Use `setState()` to update state:

```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}
```

### State Updates May Be Asynchronous

React may batch multiple `setState()` calls for performance:

```jsx
// ❌ Wrong - May not work as expected
this.setState({
  count: this.state.count + 1
});

// ✅ Correct - Use updater function
this.setState((prevState) => ({
  count: prevState.count + 1
}));
```

### State Updates are Merged

When you call `setState()`, React merges the object you provide into the current state:

```jsx
constructor(props) {
  super(props);
  this.state = {
    posts: [],
    comments: []
  };
}

componentDidMount() {
  // This only updates posts, comments remain unchanged
  this.setState({ posts: response.posts });
}
```

---

## Event Handling

Handling events in React is similar to handling events in DOM elements, with some syntax differences.

### Basic Event Handling

```jsx
function Button() {
  function handleClick() {
    alert('Button clicked!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

### Event Handling in Class Components

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOn: true };
    
    // Binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isOn: !prevState.isOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

### Arrow Functions (No Binding Required)

```jsx
class Toggle extends React.Component {
  state = { isOn: true };

  handleClick = () => {
    this.setState(prevState => ({
      isOn: !prevState.isOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

### Passing Arguments to Event Handlers

```jsx
function ActionButton({ id, name }) {
  const handleClick = (id, event) => {
    console.log(`Clicked button ${id}`, event);
  };

  return (
    <button onClick={(e) => handleClick(id, e)}>
      {name}
    </button>
  );
}
```

### Preventing Default Behavior

```jsx
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('Form submitted');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

---

## Conditional Rendering

You can create distinct components and render only some of them depending on the state of your application.

### Using if-else

```jsx
function Greeting({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  }
  return <h1>Please sign up.</h1>;
}
```

### Using Ternary Operator

```jsx
function Greeting({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? (
        <h1>Welcome back!</h1>
      ) : (
        <h1>Please sign up.</h1>
      )}
    </div>
  );
}
```

### Using Logical && Operator

```jsx
function Mailbox({ unreadMessages }) {
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 && (
        <h2>You have {unreadMessages.length} unread messages.</h2>
      )}
    </div>
  );
}
```

### Preventing Component from Rendering

Return `null` to hide a component:

```jsx
function WarningBanner({ warn }) {
  if (!warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```

---

## Lists and Keys

You can build collections of elements and include them in JSX using curly braces `{}`.

### Rendering Multiple Components

```jsx
const numbers = [1, 2, 3, 4, 5];

function NumberList() {
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );

  return <ul>{listItems}</ul>;
}
```

### Keys

Keys help React identify which items have changed, are added, or are removed:

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          {todo.text}
        </li>
      ))}
    </ul>
  );
}
```

**Important Key Rules:**
- Keys must be unique among siblings
- Keys should be stable (don't use array index if items can be reordered)
- Keys are not passed as props to components

### Using Index as Key (Only as Last Resort)

```jsx
// ⚠️ Only use index as key if items have no stable IDs
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
);
```

### Extracting Components with Keys

Keys should be specified on the component in the array:

```jsx
// ✅ Correct - Key on the component in the array
function ListItem({ value }) {
  return <li>{value}</li>;
}

function NumberList({ numbers }) {
  return (
    <ul>
      {numbers.map((number) => (
        <ListItem key={number.toString()} value={number} />
      ))}
    </ul>
  );
}
```

---

## Forms

HTML form elements work differently from other DOM elements in React because form elements naturally keep some internal state.

### Controlled Components

In HTML, form elements maintain their own state. In React, we make React state the "single source of truth":

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { value: '' };
  }

  handleChange = (event) => {
    this.setState({ value: event.target.value });
  }

  handleSubmit = (event) => {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input 
            type="text" 
            value={this.state.value} 
            onChange={this.handleChange} 
          />
        </label>
        <button type="submit">Submit</button>
      </form>
    );
  }
}
```

### Textarea

```jsx
class EssayForm extends React.Component {
  state = {
    value: 'Please write an essay about your favorite DOM element.'
  };

  handleChange = (event) => {
    this.setState({ value: event.target.value });
  }

  handleSubmit = (event) => {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Essay:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <button type="submit">Submit</button>
      </form>
    );
  }
}
```

### Select

```jsx
class FlavorForm extends React.Component {
  state = { value: 'coconut' };

  handleChange = (event) => {
    this.setState({ value: event.target.value });
  }

  handleSubmit = (event) => {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <button type="submit">Submit</button>
      </form>
    );
  }
}
```

### Multiple Inputs

```jsx
class Reservation extends React.Component {
  state = {
    isGoing: true,
    numberOfGuests: 2
  };

  handleInputChange = (event) => {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange}
          />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange}
          />
        </label>
      </form>
    );
  }
}
```

---

## Composition vs Inheritance

React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.

### Containment

Some components don't know their children ahead of time. Use the special `children` prop:

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">Welcome</h1>
      <p className="Dialog-message">Thank you for visiting!</p>
    </FancyBorder>
  );
}
```

### Multiple "Slots"

Sometimes you need multiple "holes" in a component:

```jsx
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={<Contacts />}
      right={<Chat />}
    />
  );
}
```

### Specialization

Sometimes we think about components as being "special cases" of other components:

```jsx
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">{props.title}</h1>
      <p className="Dialog-message">{props.message}</p>
      {props.children}
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!"
    />
  );
}

function SignUpDialog() {
  return (
    <Dialog title="Mars Exploration Program" message="How should we refer to you?">
      <input placeholder="Email" />
      <button>Sign Me Up!</button>
    </Dialog>
  );
}
```

### Composition Works for Class Components Too

```jsx
class SignUpDialog extends React.Component {
  state = { login: '' };

  handleChange = (e) => {
    this.setState({ login: e.target.value });
  }

  handleSignUp = () => {
    alert(`Welcome aboard, ${this.state.login}!`);
  }

  render() {
    return (
      <Dialog 
        title="Mars Exploration Program"
        message="How should we refer to you?"
      >
        <input
          value={this.state.login}
          onChange={this.handleChange}
        />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }
}
```

---

## Additional Resources

- [Official React Documentation](https://react.dev/)
- [React Tutorial](https://react.dev/learn)
- [Thinking in React](https://react.dev/learn/thinking-in-react)

---

## Practice Exercises

1. **Create a Counter Component**: Build a counter that increments and decrements
2. **Todo List**: Create a simple todo list with add and delete functionality
3. **User Profile Card**: Display user information passed as props
4. **Temperature Converter**: Convert between Celsius and Fahrenheit
5. **Filterable Product List**: Create a list of products with search functionality

Happy Learning! 🚀