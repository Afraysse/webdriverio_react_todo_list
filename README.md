# Intro
This is a simple React Todo list app, which uses Redux as a state management system.

### Test Driven Development
This app was created with a mind toward **TDD** (Test Driven Development). The end-to-end (e2e) tests use **webdriverIO** and unit tests are created using **enzyme**.

What's more, I'm going to use **linting** to enforce style rules, to remove unneeded code and decreasing cognitive load. Following the most relevant and popular choice, I'll be using `eslint` with `airbnb` rules.

```
npm install --save-dev eslint
node_modules/.bin/eslint --init
```

After running the last command, you should be faced with a short series of questions. With regards to answering the question, `How would you like to configure ESLint?` I put the following:

* `Use a popular style guide` - `Enter`
* `Which style guide do you want to use?` - `airbnb`
* `Do you use React` - `y` (obvs)
* `What format do you want your config file to be in?` - `JavaScript`

To learn more about **ESLint**: https://www.npmjs.com/package/eslint

### Accommodating for JSX in .js with ESLint
Since ESLint will complain when we write JSX in a `.js` file, we're going to write an exception to the rules.

What is JSX? Just to review, JSX (JavaScript with XML) allows us to write concise and easily readable React code, including standard html in our JavaScript.

Having installed eslint, you should now see an `.eslintrc.js` in the base of your file app file hierarchy. Add the following rule:

```
"rules": {
  "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"]}]
},
```

### Using ESLint to check files
As mentioned, ESLint can be used to enforce style rules and in general, enforce best practices for writing successful code. In order to check up on files within your app, you can run the following code:

`$ ./node_modules/.bin/eslint yourfile.js`

Let's check up on our `App.js` file.

`$ ./node_modules/.bin/eslint src/App.js`

When I run this on my `src/App.js` I receive the following error:


ESLint wants us to write it as a *stateless* pure function as opposed to the *stateful* React component it is currently.

```
// A stateful React component

class App extends Component {
  render() {
    return (
      <div>My App</div>
    );
  }
}

export default App
```

All full-on React component gives you access to all the lifecycle methods and the ability to store and mutate the state but it's not really the nicest way to write code, if we can avoid it.

**Pure functions** have the bonus of being:
* Really nice to test
* Always have predictable returns
* Are more performant
* Are written in an immutable, functional style

```
// A stateless pure function

const App = () => (
  <div>My App</div>
  )

  export default app
```

### Building Unit Tests with Enzyme
For starters, install enzyme with npm:

`$ npm install --save-dev enzyme enzyme-adapter-react-16`

Next, install the react-test-renderer dependency:

`$ npm install --save-dev enzyme react-test-renderer`

From Enzyme, we import the `shallow` module from the `enzyme` testing library.

The **shallow** rendering modal is used to constrain testing to only a component as a unit. It's also used to ensure that your tests aren't indirectly asserting on behavior of child components.

Add the following code to your test files:

**ES6**
```
// setup file
import { configure } from 'enzyme';
import Adapter from 'adapter';

configure({ adapter: new Adapter() });
```

```
// test file
import { shallow, mount, render } from 'enzyme';

const wrapper = shallow(<Foo />);
```

### End-to-End Testing
Our e2e testing solution is going to involve:
* `selenium` - letting us fire up a browser
* `webdriverIO` - for controlling it
* `chai` - granting us assertions

To install the following, run:

`$ npm install --save-dev selenium-standalone webdriverio chai`

And then perform the following setup for webdriverIO:

`node_modules/.bin/wdio config`

Once the setup command for webdriverio is run, you'll be faced with the following questions (similar to ESLint) to set up your `wdio.conf.js`.



To test with Chrome, edit the `wdio.conf.js` to select the default browser.

`browserName: chrome`

### Create Selenium scripts for current and later testing

In the `package.json`, create the following scripts to handle all e2e testing needs:

```
"scripts": {
  "selenium-setup": "selenium-standalone install",
  "selenium-start": "selenium-standalone start",
  "e2e-tests": "wdio wdio.conf.js",
  "e2e-tests-watch": "wdio wdio.conf.js --watch"
  ...
}
```

Once you've updated your `package.json`, setup and start the selenium server:

`npm run selenium-setup`

`npm run selenium-start`

### The wdio.conf.js WebdriverIO config file
The wdio.conf.js file serves as the configuration file that contains all necessary information to run your test suite.

**Word of Caution:** If this file is set up incorrectly, your **test suite** will fail. You will get frustrated. You will be angry.
