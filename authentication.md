# Authentication

- [Configuration](#configuration)
- [Methods](#methods)
    - [login](#login)
    - [register](#register)
    - [logout](#logout)
    - [user](#user)
    - [fetchUser](#fetchUser)
    - [check](#check)
    - [loggedIn](#loggedIn)
    - [guest](#guest)
    - [id](#id)
    - [guard](#guard)
    - [getToken](#getToken)
    - [getStorage](#getStorage)
    - [getHeaders](#getHeaders)
    - [onError](#onError)
- [Drivers](#drivers)
- [Middleware](#middleware)

## Configuration

./config/auth.ts

```typescript
export default {
    defaults: {
        guard: 'user'
    },
    user: {
        idPropertyName: 'id'
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
                    redirect: '/dashboard' // default false
                },
                logout: {
                    url: '/auth/logout',
                    method: 'post', // default post
                    redirect: '/home'
                },
                register: {
                    url: '/auth/register',
                    method: 'post', // default post
                    redirect: '/home', // default false
                    autoLogin: true, // default false
                }
            },
            headers: {},
            // Request
            tokenName: 'token',
            // Response
            accessTokenName: 'access_token',
            expiresAtName: 'expires_at',
            tokenType: 'bearer'
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

If you use `then` you got as response: loginResponse and userResponse.

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

### onError

**handler**: `function`


```js
this.authService.onError(handler);
```

**error**: `AxiosError`
**guard**: `string`
**entpoint**: `string`

```js
this.authService.onError((error, guard, endpoint) => {
    console.log(error, guard, endpoint)
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

