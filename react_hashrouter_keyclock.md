# React, HashRouter & Keyclock
I am documenting things that I have understood while create React application , with Hashrouter and authenticated using Keyclock.
<br/>

> 🦨 If your application is adding  `&state=`  to the url after Keyclock is initalized, then this documetation might help you 🚑....

<br/>

###  TLDR

👉 By default, when the responseMode is set to 'fragment', Keycloak adds the state parameter to the URL fragment. However, when the responseMode is set to 'query', the state parameter is added to the query string instead.

```
keycloak
    .init({ 
	onLoad: 'login-required',
	//add the below line 👇
	responseMode: 'query' 
	})
```

👉 Alternate solution, use **BrowerRouter** * without responseMode* (default to 'fragment')

<br/>

### Sample project example

**Dependencies info**
```
    "keycloak-js": "^19.0.2",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-router-dom": "^6.3.0",
    "react-scripts": "5.0.1",
    "universal-cookie": "^4.0.4"
```
**index.js**
```
import React from 'react';
import ReactDOM from 'react-dom';
import { HashRouter } from 'react-router-dom';
import App from './App';

ReactDOM.render(
  <HashRouter>
    <App />
  </HashRouter>,
  document.getElementById('root')
);

```

**App.js**
```
import './App.css';
import { Route, Routes, Navigate, Link } from 'react-router-dom';
import NotFound from './NotFound';
import components from './page';
import appRoutes from './appRoutes';

const { Login } = components;

function App() {
  return (
    <>
      <div style={{ display: 'flex', gap: '10px' }}>
        <Link to={'/login'}>Login</Link>
        <Link to={'/a'}>A</Link>
        <Link to={'/b'}>B</Link>
        <Link to={'/c'}>C</Link>
      </div>
      <hr />
      <Routes element>
        <Route path='/' element={<Navigate to={'/login'} />} />
        <Route path='login' element={<Login />} />
        {appRoutes}
        <Route path='*' element={<NotFound />} />
      </Routes>
    </>
  );
}

export default App;

```

**appRoutes.js**
```
import { Route, Outlet, useLocation, useNavigate } from 'react-router-dom';
import components from './page';
import Cookies from 'universal-cookie';
import keycloak from './keyclock';

const { A, A1, A2, A3, B, C } =
  components;

const cookie = new Cookies();

const publicPath = ['/a', '/a/a1'];

const SecureRoutes = () => {
  const isGuestUser = JSON.parse(cookie.get('isGuest'));
  const location = useLocation();
  const navigate = useNavigate();

  if (!isGuestUser) {
    keycloak
      .init({ onLoad: 'login-required', responseMode: 'query' })
      .then((authenticated) => {
        if (authenticated) {
          cookie.set('isGuest', false);
        } else {
          cookie.set('isGuest', true);
        }
      })
      .catch((error) => {
        console.error('Failed to initialize Keycloak', error);
      });
  } else if (!publicPath.includes(location.pathname)) {
    alert('As guest you cannot access this ');
    console.log({ navigate });
  }

  return (
    <>
      <div>
        <div>Is Guest:{isGuestUser ? 'yes' : 'No'}</div>
        {!isGuestUser && (
          <button
            onClick={() =>
              keycloak.logout({ 
			  //redirectUri:<URL to go when logout>
			  redirectUri: 'http://localhost:3000/'
			  })
            }
          >
            Logout
          </button>
        )}
      </div>
      <hr />
      <Outlet />
    </>
  );
};

const appRoutes = (
  <>
    <Route element={<SecureRoutes />}>
      <Route path='/a'>
        <Route index element={<A />} />
        <Route path='a1' element={<A1 />} />
        <Route path='a2' element={<A2 />} />
        <Route path='a3' element={<A3 />} />
      </Route>
      <Route path={'/b'} element={<B />} />
      <Route path='/c' element={<C />} />
    </Route>
  </>
);

export default appRoutes;

```
