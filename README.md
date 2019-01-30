# Introduction

Let's start with some wireframes for our project

![wires](https://s3.amazonaws.com/learn-site/curriculum/cat-tinder/cat-tinder-wireframe.png)

## App setup
Using create-react-app, react-router-dom and react-bootstrap, we can setup a new application:

```bash
$ create-react-app cat-tinder-frontend
$ cd cat-tinder-frontend
$ yarn add react-bootstrap react-router-dom
$ yarn add -D enzyme react-test-renderer enzyme-adapter-react-16
```


## Add a theme

I'm going to use the "United" theme from bootswatch.com, so I'll add the stylesheet to 'pubic/index.html'  You can download a theme from here: [Bootswatch](https://bootswatch.com/) and put it in the ```public/``` directory.

#### public/index.html
```
<link rel="stylesheet" href="%PUBLIC_URL%/bootstrap.min.css">
```

## Setup the router
```javascript
import React, { Component } from 'react'
import { BrowserRouter as Router, Switch, Route } from ‘react-router-dom'

import Cats from ‘./pages/Cats’
import NewCat from ‘./pages/NewCat’

class App extends Component {
  render() {
    return (
      <div>
	<Header />
	<Router>
          <Switch>
           <!-- Your Routes Here -->
	  </Switch>
        </Router>
      </div>
    );
  }
}

export default App;

```

We haven’t really built any components yet, so this code will throw an error, but we have set up our basis for handling requests.

## Challenge

Did you see that we are using a Header component that hasn't been created anywhere?

<b>Make a header component with bootstrap and add it to the project as src/components/Header.js.</b>

Remember that running ``` yarn start ``` will error out until we create the Cats and NewCat components. Feel free to put placeholders there while you create your header.

# NewCat Component

Time to build the form to add new cats. Remember we already have the route for NewCat in App.js - so all that’s left is to create the component.

## Challenge

Your challenge this time is to create a component that fulfills the tests in this file. You will see that these tests assume we are using bootstrap to create our view, and reference bootstrap components that will need to be added to your pages/Cats.js file.

#### ```src/pages/__tests__/NewCat.js```
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import NewCat from '../NewCat'
import { mount } from 'enzyme'
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16'

Enzyme.configure({ adapter: new Adapter() });

it('renders without crashing', () => {
  const div = document.createElement('div')
  ReactDOM.render(<NewCat />, div)
})

it('has a name input', ()=>{
  const component = mount(<NewCat />)
 expect(component.find('label#name').text()).toBe("Name")
})

it('has a age input', ()=>{
  const component = mount(<NewCat />)
  expect(component.find('label#age').text()).toBe("Age")
})

it('has a enjoys input', ()=>{
  const component = mount(<NewCat />)
  expect(component.find('label#enjoys').text()).toBe("Enjoys")
})

it('has a submit button', ()=>{
  const component = mount(<NewCat />)
  expect(component.find('button#submit').text()).toBe("Create Cat Profile")
})
```

Once you have a view that satisfies the tests, you’ll be ready to refactor your code to create a controlled form.

## Controlled components
Thinking ahead just a bit, we're going to need to pass the values from our form back up to the calling component. In order to do this easily, we will hold the values typed in by the user in state. To “watch” our inputs and save values into state, we need to switch our inputs to being “controlled components”(meaning watched by state). Or, in other words, add a 'value', and an 'onChange' attribute to the inputs. Then we can manage the value of the inputs in the components’ internal state until the form is submitted. Do you remember how to do this from the "Component, State, & Props" day?

We start by adding state to the component in a constructor:

#### src/pages/NewCat.js
```javascript
constructor(props){
  super(props)
  this.state = {
    form:{
      name: '',
      age: '',
      enjoys: ''
    }
  }
}
```
And then for each input, we bind its value to state. We'll add a name to the input too, and an 'onChange()' callback, as we're going to need those next. Here is 'name', the other two are nearly identical.

#### src/pages/NewCat.js
```javascript
<FormControl
  type="text"
  name="name"
  onChange={this.handleChange.bind(this)}
  value={this.state.form.name}
/>
```

So what does ```handleChange()``` look like?

#### src/pages/NewCat.js
```javascript
handleChange(event){
  let {form } = this.state
  form[event.target.name] = event.target.value
  this.setState({form: form})
}
```

## For discussion

Notice how we didn't test the 'handleChange()' method and when it was called?  Why do you suppose we didn't do that?

The answer is that ```handleChange()``` is an internal mechanism of the component, and we want to have flexibility later down the road to change how the component works. We're not particularily interested in those inner workings from a testing perspective.

What we are interested in is what the component passes back to its caller, which we're going to test extensivly.  If you remember that testing is for validating outputs based on particular inputs, you'll write flexible tests that allow you to easily refactor.

As a General Rule:
* Don't test the inner working of a component.
* Test that you get the correct outputs based on specific inputs.
* Test behavior of the component, especially if it directly affects the user experience.

# Cats Component

Its time to turn our attention to the page components of the application. We'll start with the cats index page and some fake data so that we can get the look of the page correct.

Here's the basic test to start us out:

#### ```src/pages/__tests__/Cats.js```

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import Cats from '../Cats'
import { mount } from 'enzyme'
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16'

Enzyme.configure({ adapter: new Adapter() });

it('Cats renders without crashing', () => {
  const div = document.createElement('div')
  ReactDOM.render(<Cats />, div)
})
```

That will fail until we create the component

#### src/pages/Cats.js
```javascript
import React, { Component } from 'react';
import {
  Grid, Col, Row
} from 'react-bootstrap'

class Cats extends Component {
  render() {
    return (
	<Grid>
      <Row>
        <Col>
    		<div>I'm a component</div>
        </Col>
      </Row>
	<Grid>
    );
  }
}

export default Cats;

```

Now that test should pass, because we have created a component that can be rendered. (Meaning that it imports react and has a render function, not that it shows real content.)

But lets fix that by adding some fake cat data to play with. Later this information will come from the rails backend, but for now lets just get something up that we can see and work with.

We want all of our data in a central place, so instead of placing it directly in the pages/Cats.js component, we will put it in our logic component — App.js

#### src/App.js
```javascript
constructor(props){
    super(props)
    this.state = {
      cats: [
        {
          id: 1,
          name: 'Morris',
          age: 2,
          enjoys: "Long walks on the beach."
        },
        {
          id: 2,
          name: 'Paws',
          age: 4,
          enjoys: "Snuggling by the fire."
        },
        {
          id: 3,
          name: 'Mr. Meowsalot',
          age: 12,
          enjoys: "Being in charge."
        }
      ]
    }
  }

```

Now we need to send this cats json array to the Cats component as props from App.js. To do this, we need a slightly different syntax in our Router. Change the Cats route to look like this:

``` <Route exact path="/cats" render={(props) => <Cats cats={this.state.cats}/>} /> ```

Now that our Cats.js component is receiving an array of cats in props, lets add some bootstrap code to create real content in the Cats.js render function, replacing the blank elements we had before. 

What else do you have to change about your page to make this work?

#### pages/Cats.js

```javascript
<Row>
   	<Col xs={12}>
        	<ListGroup>
            {this.props.cats.map((cat, index) =>{
              return (
                <ListGroupItem
                  key={index}
                  header={
                    <h4>
                      <span className='cat-name'>
                        {cat.name}
                      </span>
                      - <small className='cat-age'>{cat.age} years old</small>
                    </h4>
                  }>
                  <span className='cat-enjoys'>
                    {cat.enjoys}
                  </span>
                </ListGroupItem>
              )
            })}
          </ListGroup>
        </Col>
      </Row>

```

## Finishing the test

Now we get to test the information in our Cats.js component. Problem, now that the Cats.js takes in props from App.js — how can we test that? Our Cats.js component requires that information in order to render.

We need our test to send some json data to pages/Cats.js the same way that App.js is currently sending the cats json as props to pages/Cats.js. It is really convenient if our test uses the same fake data as we have in App.js state.

Below, you’ll notice that we’re using an import statement for a thing called mount from Enzyme. It will allow us to pass information to a component we are testing. 

Write some tests to cover the content we just added to Cats.js.

#### ```src/pages/__tests__/Cats.js```
```javascript
const cats = [
  {
    id: 1,
    name: 'Morris',
    age: 2,
    enjoys: "Long walks on the beach."
  },
  {
    id: 2,
    name: 'Paws',
    age: 4,
    enjoys: "Snuggling by the fire."
  },
  {
    id: 3,
    name: 'Mr. Meowsalot',
    age: 12,
    enjoys: "Being in charge."
  }
]

it('Cats renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<Cats cats={cats} />, div);
});

it('Renders the cats', ()=>{
  const component = mount(<Cats cats={cats} />)
  const headings = component.find('h4 > .cat-name')
  expect(headings.length).toBe(3)
})

```

Try those tests, they should be green.

## Challenge

Now, try adding some more tests of your own.

# NEWCAT FUNCTIONALITY

Now that the NewCat form is rendering correctly, we can wire the form up to pass submitted data to App.js, and handle errors correctly.

What happens when a user clicks "Create Cat Profile" ?

When a user clicks the “Create a Cat” button to submit the form, we want that to trigger a series of actions that lead to adding a new cat to the rails database. Lets chart where the information submitted by the user has to travel in order to get to the db. 

## Flow of information from form to DB
#### 1. Preparation

##### in App.js
A function is created to handle information for a new cat. That function is preemptively sent to the dumb component (in our case pages/NewCat.js) as props. This function is saying to the dumb component: “Here is a function that takes in new cat json as an argument. Only run it when you have all the information for a new cat”.

#### 2. Data Entry

##### in pages/NewCat.js
User inputs data into the form. The form is controlled, and saves all data directly into state

#### 3. Form Submit

##### in pages/NewCat.js
User hits submit - this is our signal that they are finished filling in their information. The form runs an onClick function, which calls the function held in props from App.js. That function runs and sends the form data (currently in state) to the logic component (in our case App.js)

#### 4. Pass Data to the API

#### in App.js
We now would trigger a POST type request to the backend, and rails would handle the rest — but more on that tomorrow. 

## Challenge

Create a ```handleNewCat``` function in App.js that only does one thing: console.log the information from the NewCat form. Pass the handleNewCat function to the NewCat component as props, and then run the handleNewCat function when a user clicks the form submit button

