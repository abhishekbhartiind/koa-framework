## *KOA*
- Koa is a new web framework designed by the team behind Express, which aims to be a smaller, more expressive, and more robust foundation for web applications and APIs. 
- Through leveraging generators Koa allows you to ditch callbacks and greatly increase error-handling. Koa does not bundle any middleware within core, and provides an elegant suite of methods that make writing servers fast and enjoyable.
- Koa aims to "fix and replace node", whereas Express "augments node".
- Koa uses `promises` and `async` functions to rid apps of callback hell and simplify error handling.
- It exposes its own `ctx.request` and `ctx.response` objects instead of node's req and res objects.
- Koa can be viewed as an abstraction of node.js's http modules, where as Express is an application framework for node.js


## How is Koa different than Connect/Express?

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

## Testings

`Supertest` is a way to call on routes, with a simple syntax.

Server is our application. (Note: This will not work until we properly export our application, which we will do in a minute)

`BeforeAll()` & `AfterAll()`

```bash
beforeAll(async () => {
 // do something before anything else runs
 console.log('Jest starting!');
});
// close the server after each test
afterAll(() => {
 server.close();
 console.log('server closed!');
});
```
These functions allow us to do things before and after all the tests run. Everything in Jest is based on async/await, so we know we can do things in the right order without weird stuff.

Typically, I use beforeAll to add things to the DB. It’s highly recommended you close the server after each test, otherwise Jest won’t be able to use its watch function appropriately.

`describe()` & `test()`

```bash
describe('basic route tests', () => {
 test('get home route GET /', async () => {
 const response = await request(server).get('/');
 expect(response.status).toEqual(200);
 expect(response.text).toContain('Hello World!');
 });
});
```
Jest breaks up the tests into chunks. It first breaks them up by file, then by blocks, then by test. You can see it visually when you run the tests in the terminal.

`describe()` is used to chunk a group of tests together. For example, in this scenario I named it ‘basic route tests’ because I’m going to test the public routes.

`test()` is used to create individual tests. Let’s take a closer look at the actual test being run.

```bash
const response = await request(server).get('/');
expect(response.status).toEqual(200);
expect(response.text).toContain('Hello World!');
```
we’re requesting the route of ‘/’ with the method of ‘GET’ from our application.

`expect().toEqual()` and `expect().toContain` are pretty self explanatory. 
We expect our response from the server to equal a set value. In this case, 200. 

```bash
app.use(async (ctx, next) => {
 try {
 await next();
 } catch (err) {
 ctx.status = err.status || 500;
 ctx.body = err.message;
 ctx.app.emit('error', err, ctx);
 }
});
```

`app.context` is the prototype from which ctx is created. You may add additional properties to ctx by editing `app.context`. This is useful for adding properties or methods to ctx to be used across your entire app, which may be more performant (no middleware) and/or easier (fewer require()s) at the expense of relying more on ctx, which could be considered an anti-pattern.

Basically, if we want to add a global object/method to the app, we can do it here.

Additionally, ctx serves as the wrapper for both the request and the response object.