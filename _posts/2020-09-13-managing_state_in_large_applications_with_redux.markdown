---
layout: post
title:      " Managing State In Large Applications With Redux"
date:       2020-09-13 19:07:18 -0400
permalink:  managing_state_in_large_applications_with_redux
---


When creating a large react application, managing the state of components can quickly become quite difficult and confusing. This is because oftentimes we need to pass the current state of a component to another (oftentimes not the parent component) to make updates within our application. Luckily, there's a JavaScript library called Redux that we can import that will manage the state of the entire application for us. The initial setup is tedious, but completely worth it in the end. I will be going over how I set it up in my final project. More specifically will dive into setting the initial state and then updating it after a user logs in.

After creating a new react app, the first thing you're going to want to do is import Redux and other relative libraries with these individual command line prompts (alternatively you can use npm install --save in place of yarn add):

1. `yarn add redux`

2. `yarn add react-redux`

react redux the official react binding for redux. It lets your react components read data from a redux store and dispatch actions to the store to update data (more on this later.)

3. `yarn add redux-thunk`

Actions are plain old JavaScript objects. Redux-thunk is a middleware that give actions the ability the pass functions, then looks at every action that passes through the system, and if it's a function, it calls that function. More on this [here](http://github.com/reduxjs/redux-thunk).


Once these libraries have been added to your package.json file, it's time to import them in your index component.

```
// ./src/index.jsx


import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';

import reducers from './reducers/rootReducer';

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(reducers, composeEnhancers(applyMiddleware(thunk)));

//import * as serviceWorker from './serviceWorker';

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

In the above code snippet, it's important to note that I'm doing that a few things here. First, I am importing my libraries and creating a store variable that takes in the reducer as the first argument. The second argument is accessing the browser to find a method called REDUX_DEVTOOLS_EXTENSION__. This communicates with a chrome extension that allows you to have an in browser view of the store and each action that is dispatched. More on this [here](http://chrome.google.com/webstore/detail/redux-devtools/lmkhkpmbekcpmknklioeiffkpmmfibljd?hl=en). I am then wrapping the app component inside a provider component and passing the store as a prop to the app component, so is that the entire application can have access to the store (which is where our state is being managed). Whew! Brain hurting yet?

Next lets setup our reducer.

*NOTE: You don't have to do these steps in any particular order, just as long as they all get done. But I'll go through the steps in a way that I think will be easy for you to follow.*

```

// ./reducers/reducers.jsx


export default (state = {}, action) => {
    switch (action.type) {

        case 'LOGIN_USER':
            return (action.payload)

        case 'CURRENT_USER':
            return (action.user)
						
        default:
            return state;
    };
};
```

In the code above we are exporting a function - passing in the initial state as an empty object, and an action as the second argument. The function then uses conditions based on the type of action and then dispatches that action. If none of the conditions are met, we'll return the initial state.

Okay. Now let's set up an action file that will get our users from our database and another that will log them in.

```

// ./src/actions/action.jsx

//---------------------------------------------------------------------
//-----------------------------------------------------------LOGIN_USER
export const loginUser = (payload, callback) => async(dispatch) => {
    const response = await fetch("http://localhost:3000/login", {
            method: "POST",
            credentials: 'include',
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                user: payload
            })
        })
        .then(res => res.json())
        if (response.error) {
            console.log(response.error)
        }
        dispatch({type: 'LOGIN_USER', payload: response})
        callback()
};

//---------------------------------------------------------------------
//-----------------------------------GET_USER (CHECKS FOR CURRENT USER)
export const getUser = () => { 
    return dispatch => {   
        return fetch("http://localhost:3000/currentUser", {  
            credentials: "include",         
            headers: {            
                "Content-Type": "application/json"          
                }
            })        
            .then(res => res.json())        
            .then(userData => {          
                if (userData.error) {            
                    console.log(userData.error)          
                } else {            
                     dispatch({type: "CURRENT_USER", user: userData})          
                }
            })        
    }
}
```

I'm not going to go into too much detail about this part, but because we are using redux-thunk, our actions can optionally be functions. As you see above, these actions (functions) are just a simple fetch requests. But you'll notice that after we get the the responses, we grab that user data and dispatch the action types, passing in the user data as a user object into the state via the reducer. Still with me?

Sweet! If we set this up correctly, we can test it immediately after setting up a few components. So let's do that!

*NOTE: Its important to note that I am making my requests to a Rails API that I created in conjuction with my client that runs on localhost:3000.*

Next we'll take a look at the app component.


```

// ./src/App.jsx

import React from 'react';
import NavBar from './NavBar';
import {connect} from 'react-redux'
import {getUser, getWorks} from './actions/actions';


class App extends React.Component {

  componentDidMount(){
    this.props.getUser()
  }

  render(){
    return (
      <div className="App">
        <NavBar user={this.props.user}
        works={this.props.works}/>
      </div>
    ); 
  }
}

const mapState = ({user}) => {
  return {
      user
  }
}

export default connect(mapState, {getUser})(App);
```


The app component imports the getUser action from the action.jsx file, calls the action when the component mounts, then passes in the props of user to the navbar component. Additionally, and very importantly we are dispatching our action using export default connect. Using connect allows up to access our store. *We can use connect in any component that we wish to pass state to or dispatch actions to update the state.* 

Next The NavBar...

*NOTE: I used Material UI for styling.. You can ignore the stlying code if you're no familar with it (i.e. Grid, Typography, Divider, Button). *

```

// ./src/NavBar.jsx

import React from 'react';
import { BrowserRouter as Router, Route, Switch, Link} from 'react-router-dom';
import {Grid, Typography, Divider, Button} from '@material-ui/core';
import { withStyles } from '@material-ui/core/styles';
import {compose} from 'recompose';
import {connect} from 'react-redux';

import Home from './components/Home';


const styles = () => ({
    navLogo: {
      color: 'black',
      textDecoration: 'none',
      fontSize: '40px',
      fontWeight: 'bold'
    },
    naviBar: {
        color: 'black',
        textDecoration: 'none',
        fontSize: '40px',
        fontWeight: 'bold'
      },
  });


class NavBar extends React.Component {
    
    constructor(props) {
    super(props);
    this.state = {};
  }


  render(){
    const {classes} = this.props;
    return (
      <div>
          <Router>
                    <Grid container direction="column" className={classes.naviBar}>
                        <Grid item xs={12} container >
                        
                            <Grid item md={1}> </Grid>


                            <Grid item xs={6} sm={5} md={3} lg={3}>
                            <Typography className={classes.navLogo} variant='h5'>Portfoli-Yo!</Typography>
                            </Grid>

                            <Grid item sm={1} md={1} lg={5}> </Grid>

                            <Grid item xs={2} sm={2} md={2} lg={1}> 
                                <Button fullWidth color='primary' component={Link} to="/">Home</Button>
                            </Grid>

                            <Grid item xs={2} sm={2} md={2} lg={1}>
                               <Button fullWidth color='primary' component={Link} to="/about">About</Button>
                            </Grid>

                            <Grid item xs={2} sm={2} md={2} lg={1}>
                                <Button fullWidth color='primary' variant="contained" component={Link} to="/login">Login</Button>
                            </Grid>       
                        </Grid>
                        <Divider/>
                    </Grid>
                    <Switch>
                        <Route path="/login" exact component={Login} />
                        <Route path="/about" exact component={About} />
                        <Route path="/" exact component={Home} />
                    </Switch>
            </Router>
      </div>
    );
  }
}

const mapState = ({user}) => {
    return {
        user
    }
}

export default compose(
  withStyles(styles, {name: 'App',}),
  connect(mapState))(NavBar);
```

My navbar component uses the react-router-dom library to set up client side routing. As you can see above our home component will render at the URLs index *(localhost:3001/)* and the login component at the url extention "/login". Also see that here we are mapping the state and passing it in our export default connect. 

Note: I needed to import an additional library "recompse" so that I could export the App component with the Maaterial UI styling and connect. Im most cases your export would look like this: 

`export default connect(mapState)(NavBar)`

Okay, next let's take a look at the Login component.

```

./src/components/Login.jsx

import React from 'react';
import {Container, Avatar, Button, CssBaseline, TextField, FormControlLabel, Checkbox, Link, Box, Grid, Typography} from '@material-ui/core';
import LockOutlinedIcon from '@material-ui/icons/LockOutlined';
import { withStyles } from '@material-ui/core/styles';
import {compose} from 'recompose';

import Copyright from './Copyright'

import { connect } from 'react-redux';
import { loginUser } from '../actions/actions';

const styles = (theme) => ({
  paper: {
    marginTop: theme.spacing(8),
    display: 'flex',
    flexDirection: 'column',
    alignItems: 'center',
  },
  avatar: {
    margin: theme.spacing(1),
    backgroundColor: theme.palette.secondary.main,
  },
  form: {
    width: '100%', // Fix IE 11 issue.
    marginTop: theme.spacing(1),
  },
  submit: {
    margin: theme.spacing(3, 0, 2),
  },
  
});



class Login extends React.Component {
  
  state = {
    email: "",
    password: ""
}

  onInputChange = (event) => {
    this.setState({
        [event.target.name]: event.target.value
    })
  };

  onFormSubmit = (event) => {
    event.preventDefault();
    this.props.loginUser(this.state, () => {
      if(this.props.user.name){
        this.props.history.push("/home");
      }
    });

    this.setState({
        email: "",
        password: ""
    });
  };


  render(){
    const {classes} = this.props;
    // console.log(this.props)
    return (
      <Container component="main" maxWidth="xs">
        <CssBaseline />
        <div className={classes.paper}>
          <Avatar className={classes.avatar}>
            <LockOutlinedIcon />
          </Avatar>
          <Typography component="h1" variant="h5">
            Sign in
          </Typography>
          <form className={classes.form} onSubmit={this.onFormSubmit}>
            <TextField onChange={this.onInputChange}
              required
              id="email"
              label="Email Address"
              name="email"
							/>
							
            <TextField onChange={this.onInputChange}
              required
              name="password"
              label="Password"
              type="password"
              id="password"
            />
           
            <Button
              type="submit"
                         variant="contained"
              color="primary"
              className={classes.submit}
            >
              Sign In
            </Button>

          </form>

    );
  }
}

const mapState = ({user}) => {
  return {user}
}

export default compose(
  withStyles(styles, {
      name: 'App',
  }),
  connect(mapState, {loginUser}),
)(Login);
```

In this component you'll notice some similarities in regards to importing the needed actions and also mapping the state and dispatch and the export connect.
Additionally in this component, we set the initial state as an object with key pair values that have a value(s) of an empty string. In the render we are returning a form that will update that components state onChange, then save it in the store onSubmit.

If everything is coded correctly, then you would have successfully set up redux and can now see the store updated in the redux devtools. Additionally you now have access the props of the store by mapping the state to that specific component(s) you want. 

Hopefully this was hepful to you!
Happy coding!
