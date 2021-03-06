# Http Requests

- [Requests](#requests)
- [Request Middleware](#request-middleware)
- [Unregister App Middleware for a route](#unregister-app-middleware-for-a-route)

Varie uses [axios](https://github.com/axios/axios) as its default HTTP library,
and can be easily configured to meet your needs in the config.

## Requests

To make a request you will need to inject the `$http` service.

```js
$http.get("url", config);
$http.head("url", config);
$http.options("url", config);
$http.put("url", data, config);
$http.patch("url", data, config);
$http.post("url", data, config);
$http.delete("url", config);
```

Each returns a promise so you can get the response :

```js
$http.get("url").then(response => {
  this.data = response.data;
});
```

## Request Middleware

Request middleware help to intercept requests going out of your application
to provide additional features. It could be adding API tokens, or having
a localized loader.

### Request Middleware

Start by using the Varie CLI

`$ varie make:request-middleware your-middleware-name`

You have availability to customize how you want the request / response and what the responseError should do if it does indeed fail.
You can take a look at the included middleware `app/request-middleware/LoadingScreen.ts` for a example of what you might do.

### Registering Middleware

To register you will need to open the `index.ts` file inside the middleware folder and add it in the correct spot.
This allows you to customize the order of your middleware.

## Unregister App Middleware for a route

To disable a app middleware for a particular route you can use the unregistere method

```js
import MiddlewareClass from "app/middleware/MiddlewareClass";

$http
  .unregisterMiddleware(MiddlewareClass)
  .get("url")
  .then(response => {
    this.data = response.data;
  });
```
