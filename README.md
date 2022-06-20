## *KOA*
------------------------------------------------
- Koa is a new web framework designed by the team behind Express, which aims to be a smaller, more expressive, and more robust foundation for web applications and APIs. 
- Through leveraging generators Koa allows you to ditch callbacks and greatly increase error-handling. Koa does not bundle any middleware within core, and provides an elegant suite of methods that make writing servers fast and enjoyable.
- Koa aims to "fix and replace node", whereas Express "augments node".
- Koa uses `promises` and `async` functions to rid apps of callback hell and simplify error handling.
- It exposes its own `ctx.request` and `ctx.response` objects instead of node's req and res objects.
- Koa can be viewed as an abstraction of node.js's http modules, where as Express is an application framework for node.js


## How is Koa different than Connect/Express?
------------------------------------------------

- Promises-based control flow

1. No callback hell.
2. Better error handling through try/catch.
3. No need for domains.

- Koa is barebones

1. Unlike both Connect and Express, Koa does not include any middleware.
2. Unlike Express, routing is not provided.
3. Unlike Express, many convenience utilities are not provided. For example, sending files.
4. Koa is more modular.

- Koa relies less on middleware

1. For example, instead of a "body parsing" middleware, you would instead use a body parsing function.

- Koa abstracts node's request/response

1. Less hackery.
2. Better user experience.
3. Proper stream handling.
  
- Koa routing (third party libraries support)


## What I love about it

1. The whole framework is built on async/await
2. It brings practically nothing other than basic routing, so that makes it incredibly light-weight.
3. It was built by the express people as a replacement.
4. Simplest error Logging I’ve ever seen.

## Code Snippets

```bash
npm install koa
npm install koa-router
npm install koa-logger
```

```bash
const Koa = require('koa');
const Router = require('koa-router');
const logger = require('koa-logger');
const app = new Koa();
const router = new Router();
app.use(logger());
router.get('/', (ctx, next) => {
 ctx.body = 'Hello World!';
});
app.use(router.routes());
app.use(router.allowedMethods());
app.listen(3000);
```

```bash
npm install -D jest supertest
```