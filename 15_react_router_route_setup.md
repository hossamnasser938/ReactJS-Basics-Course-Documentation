# React Router - Route Setup
* For **ReactJS** to be able to handle a *single-page application*, it needs additional **tool** for managing URLs entered by the user or generated by hitting a link and load the *components* suitable for each URL because in a *single-page application* the page changes are handled by the front-end application not the server.
* Here comes ` Router ` to do that for us.
* To install `Router ` in your project hit ` npm install --save react-router `.
* Before we start using ` Router ` let's modify two *components* that we're gonna use:
    * ` Header `: which will contain a ` nav ` bar used to navigate between pages in the application.
    ```
    import React from 'react';
    const Header = (props) => {
        return(
            <nav>
              <ul>
                <li><a href="">Home</a></li>
                <li><a href="">Person</a></li>
              </ul>
            </nav>
        );  
    };
    ```
    * ` Person `: which will be a basic page displaying the id of the current person.
    ```
    import React from 'react';
    class Person extends React.Component {
        render() {
            return (
                <div>
                  <h2>Person Page</h2>
                  <p>Person ID: </p>
                </div>
            );
        }
    }
    ```
* We start using the ` Router ` in the entry point of our application. So in ` AppComponent `:
```
import React from 'react';
import { render } from 'react-dom';
import { Router, Route } from 'react-router';
import { Home } from 'Home';
import { Person } from 'Person';
class AppComponent extends React.Component {
    render() {
        return(
            <Router>
              <Route path="home" component={ Home }/>
              <Route path="person" component={ Person }/>
            </Router>
        );
    }
}
render( <AppComponent/>, window.document.getElementByID( "app" ) );
```
    * from ` 'react-router' ` we imported ` Router `: Which will be the parent of all pages(routes) and ` Route ` which is used to define a page.
    * For each ` Route ` we set two attributes:
        * ` path ` which defines the URL for this page. For example if our website is name ` www.example.com ` the first ` Route ` which has a ` path ` ="home" can be reached using the URL: ` www.example.com/home `.
        * ` component ` which is the component to be rendered with this URL.
* We have two forms of URLs: [We will use ` www.example.com/home ` to illustrate that]:
    * ` www.example.com/#/home `(default form).  
    What is this *hashtag* **#**?  This is used to tell the browser that this is a *single-page application* so you should handle only this part ` www.example.com ` and the rest will be handled by the *front-end application*. This is because on typing or navigating to ` www.example.com/home ` this route will be forwarded to the server and then the server will replay with error **404 (Not Found)** since it does not have this ` Route `(We handle that using the *front-end application*).  
    * ` www.example.com/home `.  
    This is a more user-friendly form that is now supported by browsers. However, we need to tell both the browser and the server that we use this form. To do that:
        * On the **browser** you need to ` import ` ` browserHistory ` from ` 'react-router' `.
        ```
        import { Router, Route, browserHistory } from 'react-router';
        ```
        and set the ` history ` attribute in the ` Route ` to this ` browserHistory `.
        ```
        <Router history={ browserHistory }>
        ```
        * On the **server** we need to configure it to support this form. In our case we use ` webPack ` server all we need to do is to head to ` package.json ` file and add this ` --history-api-fallback ` to the ` buils ` script.
* Now we need to display the ` Header ` ` Component ` on top of our application pages. So let's create a ` Component ` named ` Root ` that renders the ` Header ` component and whatever other ` Component ` given to it as a *child* using ` props.children `.
```
import React from 'react';
import { Header } from 'Header';
const Root = (props) => {
    return (
        <Header/>
        { this.props.children }
    );
}
```
* Now let's update our ` Router ` to wrap our Routes in a ` Root ` ` Component `.
```
import React from 'react';
import { render } from 'react-dom';
import { Router, Route, browserHistory } from 'react-router';
import { Root } from 'Root';
import { Home } from 'Home';
import { Person } from 'Person';
class AppComponent extends React.Component {
    render() {
        return(
            <Router history={ browserHistory }>
              <Route path="/" component={ Root }>
                <Route path="home" component={ Home }/>
                <Route path="person" component={ Person }/>
              </Route>
            </Router>
        );
    }
}
render( <AppComponent/>, window.document.getElementByID( "app" ) );
```
Notice that we nested ` Home ` and ` Person ` routes in the ` Root ` which we specified the ` path ` attribute to be **/** so the ` path ` of ` Home ` will be **/home** and ` Person ` **/person**.
* Now what is displayed when the user hit ` www.example.com/ ` without ` home ` nor ` person`? Only the ` Header ` ` Component `. Now we need to display a ` Route ` as the default. We make that using the ` IndexRoute ` which we should ` import ` from ` 'react-router' ` in ` AppComponent `.
```
import { Router, Route, browserHistory, IndexRoute } from 'react-router';
```  
and change our ` render ` method to be:
```
render() {
    return (
        <Router history={ browserHistory }>
          <Route path="/" component={ Root }>
            <IndexRoute component={ Home }/>
            <Route path="home" component={ Home }/>
            <Route path="person" component={ Person }/>
          </Route>
        </Router>
    );
}
```
Notice that in ` IndexRoute ` we specify only the ` component ` attribute no need gor the ` path ` attribute.