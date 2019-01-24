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
