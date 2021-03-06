# Environment Configuration

- [Environment Variables](#environment-variables)
- [Accessing the Configuration](#accessing-the-configuration)
- [Path Aliases](#path-aliases)

Most likely you will have multiple environments for the application `dev`, `qa`, `prod`.
You may want to change some of configuration based on what type of environment your running in.

In your root directory you will find a `.env-example`. This file
should be copied over into `.env` if not already created. In that file
you are able to customize per environment.

## Environment Variables

You can pass environment variables through webpack and can access them easily with :

```js
let version = $config.get("app.version");
```

## Accessing the Configuration

You can easily access the configuration values by using our help `$config`

```js
let value = $config.get("app.name");
```

You can also easily set the value

```js
$config.set("app.name", "My New App Name!");
```

## Path Aliases

Varie comes with some path aliases to help out with development.
These are customizable but cannot be removed as internal and plugin providers may use these.

```js
 "@app": path.join(__dirname, "app"),
 "@routes": path.join(__dirname, "routes"),
 "@config": path.join(__dirname, "config"),
 "@store": path.join(__dirname, "app/store"),
 "@models": path.join(__dirname, "app/models"),
 "@resources": path.join(__dirname, "resources"),
 "@views": path.join(__dirname, "resources/views"),
 "@components": path.join(__dirname, "app/components")
```

To customize the aliases, change the values in the `webpack.config.js` and `tsconfig.json` files.
