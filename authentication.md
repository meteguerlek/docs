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
    - [guard](#guard)
- [Drivers](#drivers)
- [Middleware](#middleware)

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
                },
                logout: {
                    url: '/auth/logout',
                    method: 'post', // default post
                },
                register: {
                    url: '/auth/register',
                    method: 'post', // default post
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

### Login

**data**: `object`
**guard**: `string`
**return**: `Promise`
```js
this.$auth.login(data, guard)
```

#### Example

```js

import Vue from "vue";

export default Vue.extend({
    $inject: ["$auth"],
    data() {
        return {
            email: '',
            password: ''
        }
    },
    methods: {
        login() {
            this.$auth.login({
                email: this.email,
                password: this.password
            })
                .then(response => {
                    this.$router.push({name: 'home'});
                })
        }
    }
});

```

### Register

**data**: `object`
**guard**: `string`
**return**: `Promise`

```js
this.$auth.register(data, guard)
```

```js
this.$auth.register({
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
this.$auth.logout(data, guard)
```

```js
this.$auth.logout()
    .then(response => {
        this.$router.push({name: 'home'});
    })

```
### user

**data**: `object`
**guard**: `string`
**return**: `object`

```js
this.$auth.user(data, guard)
```

```js
this.$auth.user();
```    
### fetchUser

**guard**: `string`
**return**: `object`

```js
this.$auth.fetchUser(guard)
```

```js
this.$auth.fetchUser();
```   

### check

Check with expires at time

**guard**: `string`
**return**: `boolean`

```js
this.$auth.check(guard)
```

```js
this.$auth.check();
```   

### loggedIn

**guard**: `string`
**return**: `boolean`

```js
this.$auth.loggedIn(guard)
```

```js
this.$auth.loggedIn();
```   

### guard

```js
this.$auth.guard('admin').login({
    emai: '...',
    password: '...'
},)
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
    protected $auth;

    constructor(@inject("$auth") auth: AuthServiceInterface) {
        this.$auth = auth;
    }

    handler(to, from, next) {

        if (this.$auth.check()) {
            return next();
        }

        return next({name: 'login'})
    }
}
````

