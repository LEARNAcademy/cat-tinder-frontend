# Setup the router
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

Next is setting up a Cats Component.

[Go to Cats Component](../02-cats-component/README.md)


[Back to Intro](../README.md)   