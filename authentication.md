# Authentication

- [Configuration](#configuration)
- [Methods](#methods)
    - [login](#login)
    - [register](#register)
    - [logout](#logout)
    - [user](#user)
    - [fetchUser](#fetchuser)
    - [check](#check)
    - [loggedIn](#loggedin)
    - [guest](#guest)
    - [id](#id)
    - [guard](#guard)
    - [getToken](#gettoken)
    - [getStorage](#getstorage)
    - [getHeaders](#getheaders)
    - [authConfig](#authconfig)
    - [redirect](#redirect)
- [Listeners](#listeners)
    - [onError](#onerror)
    - [onUnauthorized](#onunauthorized)
- [Drivers](#drivers)
- [Middleware](#middleware)
- [Use guard with httpService](#use-guard-with-httpservice)

## Configuration

./config/auth.ts

```typescript
export default {
    defaults: {
        guard: 'user'
    },
    guards: {
        user: {
            driver: 'jwt',
            endpoints: {
                user: {
                    url: '/auth/me',
                    method: 'post', // default post
                },
                login: {
                    url: '/auth/login',
                    method: 'post', // default post
                    redirect: {name: 'home'} // or '/', default false
                },
                logout: {
                    url: '/auth/logout',
                    method: 'post', // default post
                    redirect: {name: 'home'} // default false
                },
                register: {
                    url: '/auth/register',
                    method: 'post', // default
                    redirect: {name: 'home'}, // default false
                    autoLogin: false, // default false
                }
            },
            headers: {}, // default
            // Request
            tokenName: 'token', // default
            // Response
            accessTokenName: 'access_token', // default
            expiresAtName: 'expires_at', // default
            tokenType: 'bearer', // default
            idName: 'id' // default id
        },
        /*admin: {
            driver: 'jwt',
        }*/
    }
};
```

## Methods

### login

**data**: `object`
**guard**: `string`
**return**: `Promise`
```js
this.authService.login(data, guard)
```

`then()` response: 

```js 
{loginResponse: AxiosResponse, userResponse: AxiosResponse}

```

#### Example

```js

import Vue from "vue";

export default Vue.extend({
    $inject: ["authService"],
    data() {
        return {
            email: '',
            password: ''
        }
    },
    methods: {
        login() {
            this.authService.login({
                email: this.email,
                password: this.password
            })
                .then(response => {
                    console.log('Welcome '+ response.userResponse.data.name);
                    this.$router.push({name: 'home'});
                })
        }
    }
});

```

### register

**data**: `object`
**guard**: `string`
**return**: `Promise`

```js
this.authService.register(data, guard)
```

```js
this.authService.register({
    email: this.email,
    password: this.password,
    password_repeat: this.passwordRepeat,
})
    .then(response => {
        this.$router.push({name: 'login'});
    })

```

### logout

**data**: `object`
**guard**: `string`
**return**: `Promise`

```js
this.authService.logout(data, guard)
```

```js
this.authService.logout()
    .then(response => {
        this.$router.push({name: 'home'});
    })

```
### user

**data**: `object`
**guard**: `string`
**return**: `object`

```js
this.authService.user(data, guard)
```

```js
this.authService.user();
```    
### fetchUser

**guard**: `string`
**return**: `object`

```js
this.authService.fetchUser(guard)
```

```js
this.authService.fetchUser();
```   

### check

Check with expires at time

**guard**: `string`
**return**: `boolean`

```js
this.authService.check(guard)
```

```js
this.authService.check();
``` 

### loggedIn

**guard**: `string`
**return**: `boolean`

```js
this.authService.loggedIn(guard)
```

```js
this.authService.loggedIn();
```   

### guest

**guard**: `string`
**return**: `boolean`

```js
this.authService.guest(guard)
```

```js
this.authService.guest();
```   

### id

**guard**: `string`
**return**: `number|null`

```js
this.authService.id(guard)
```

```js
this.authService.id();
```   

### guard

```js
this.authService.guard('admin').login({
    emai: '...',
    password: '...'
})
```

### getToken

**guard**: `string`
**return**: `string`

```js
this.authService.getToken(guard);
```

```js
this.authService.getToken();
```

### getStorage

**guard**: `string`
**return**: `object|null`

```js
this.authService.getStorage(guard);
```

```js
this.authService.getStorage();
```

### getHeaders

**guard**: `string`
**return**: `object|null`

```js
this.authService.getHeaders(guard);
```

```js
this.authService.getHeaders();
```

### authConfig

**config**: `string`

```js
this.authService.authConfig(config);
```

```js
this.authService.authConfig('guards');
```

### redirect

**route**: `object|string|boolean`

```js
this.authService.redirect(route);
```

```js
this.authService.redirect('/'); // or { name: 'home' }
```

Like

```js
this.$router.push('/');
```

## Listeners

### onError

**handler**: `function`

```js
this.authService.onError(handler);
```

**error**: `AxiosError`
**guard**: `string`

[Handling Errors](https://github.com/axios/axios#handling-errors)

```js
this.authService.onError((error, guard) => {
    console.log(error, guard)
});
```

### onUnauthorized 

**handler**: `function`

```js
this.authService.onUnauthorized(handler);
```

**error**: `AxiosError`
**guard**: `string`

```js
this.authService.onUnauthorized((error, guard) => {
    console.log(error, guard)
});

```
## Drivers

### JWT

[JSON Web Tokens](https://jwt.io/) are an open, industry standard RFC 7519 method for representing claims securely between two parties.

## Middleware

````typescript
import {inject, injectable} from "inversify";
import RouteMiddlewareInterface from "varie/lib/routing/RouteMiddlewareInterface";
import AuthServiceInterface from "varie/lib/auth/AuthServiceInterface";

@injectable()
export default class Auth implements RouteMiddlewareInterface {
    protected authService;

    constructor(@inject("authService") auth: AuthServiceInterface) {
        this.authService = auth;
    }

    handler(to, from, next) {

        if (this.authService.check()) {
            return next();
        }

        return next({name: 'login'})
    }
}
````

## Use guard with `httpService`

We use the default guard on every request if user logged in and send the `Authorization` header:

```
{ 'Authorization' : tokenType + token }
```

If you want use other guard for a request, add the guard property into config object

### Request

```typescript
this.httpService.request({
    guard: 'admin',
    url: '...',
    params: {...}
})
```

### POST | PUT | PATCH

```typescript
this.httpService.post(url, data, {
    guard: 'admin',
})
```

### GET | DELETE | HEAD | OPTIONS

```typescript
this.httpService.get(url, {
    guard: 'admin',
    params: {...}
})
```
