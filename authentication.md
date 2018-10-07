# Authentication

## Configuration

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

## Login

**data**: `object`
**guard**: `string`
**return**: `Promise`
```js
this.$auth.login(data, guard)
```

### Example

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

## Register

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

## Logout

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
## user

**data**: `object`
**guard**: `string`
**return**: `object`

```js
this.$auth.user(data, guard)
```

```js
this.$auth.user();
```    
## fetchUser

**guard**: `string`
**return**: `object`

```js
this.$auth.fetchUser(guard)
```

```js
this.$auth.fetchUser();
```   

## check

**guard**: `string`
**return**: `boolean`

```js
this.$auth.check(guard)
```

```js
this.$auth.check();
```   
