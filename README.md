# HTTP-Modular

A universal library for converting server-side functions into ES Modules.

<img src="https://aircode-yvo.b-cdn.net/resource/modules-9sfv4swzvco.svg" width="200">

**HTTP-Modular is a compact RPC library that unlocks the full potential of BFF (Back end for FrontEnd) capabilities.**

Gone are the days of cumbersome code like this:

```js
// server-side
app.post('/save', async (context) => {
  ......
  const data = await getData(context);
  const result = await db.save(data);
  context.body = result;
});
```

```js
// in browser
...
const res = await fetch('https://<server.url>:<port>/save', {
  method: 'POST',
  body: JSON.stringify(data),
  headers: {
    ...
  }
);
const result = await res.json();
...
```

Embrace the future of coding with modular capabilities:

```js
// server-side
...
async function save(data) {
  ......
  return await db.save(data);
}
app.all('/action', modular({save, list, delete}, config.koa));
```

```js
// in browser
import {save, list, delete} from 'https://<server.url>:<port>/action';
...
const result = await save(data); // done!
...
```

### Explore the Online Demo

Server: Check out [the GitHub repository](https://github.com/AirCodeLabs/aircode/tree/main/examples/modular-demo
)

Client: Experiment with the library on [CodePen](https://codepen.io/akira-cn/pen/mdQYvmz)

## Features

- 🧸 Lightweight and user-friendly design.
- 🌎 Compatible everywhere: Supports a wide range of Node.js HTTP servers and cloud environments.

  HTTP-Modular seamlessly integrates with various environments using dedicated configurations for:
  
  - [x] [Koa](https://koajs.com/) - [Integrating with Koa](#1-integrating-with-koa)
  - [x] [Express](https://expressjs.com/) - [Integrating with Express](#2-integrating-with-express)
  - [x] [Fastify](https://fastify.dev/) - [Integrating with Fastify](#3-integrating-with-fastify)
  - [x] [Nitro](https://nitro.unjs.io/) - [Integrating with Nitro](#4-integrating-with-nitro)
  - [x] [Vercel](https://vercel.com/) - [Integrating with Vercel](#5-integrating-with-vercel-api-functions)
  - [x] [AirCode](https://aircode.io/) - [Integrating with AirCode](#6-integrating-with-aircode-cloud-functions)

- 🧩 Effortless extensibility.

  Extend HTTP-Modular to other environments like Deno, Edge Runtime, or Ben by crafting custom configurations.

## Quick Started

### 1. Integrating with Koa:

```js
import Koa from "koa";
import { bodyParser } from "@koa/bodyparser";
import { modular, context, config } from 'http-modular';

function add(x, y) {
  return x + y;
}

const getHost = context(ctx => ctx.request.hostname);

const app = new Koa();
app.use(bodyParser());

// response
app.use(modular({ add, getHost }, config.koa));

app.listen(3000);
```

### 2. Integrating with Express:

```js
import express from "express";
import bodyParser from 'body-parser';
import { modular, context, config } from 'http-modular';

const app = express();

app.use(bodyParser.json({
  limit: '4.5mb',
  type: '*/*',
}));

function add(x, y) {
  return x + y;
}

const getHost = context(ctx => ctx.request.hostname);

function getMessage() {
  return {hi: 'there'};
}

app.all('/', modular({ add, getHost, getMessage }, config.express));

app.listen(3000);
```

### 3. Integrating with Fastify:

```js
import Fastify from 'fastify';
import { modular, context, config } from 'http-modular';

const fastify = Fastify({
  logger: true
});


function add(x, y) {
  return x + y;
}

const getHost = context(ctx => ctx.request.hostname);

function getMessage() {
  return {hi: 'there'};
}

// Declare a route
fastify.all('/', modular({add, getHost, getMessage}, config.fastify));

// Run the server!
fastify.listen({ port: 3000 }, function (err, address) {
  if (err) {
    fastify.log.error(err);
    process.exit(1);
  }
  // Server is now listening on ${address}
});
```

### 4. Integrating with Nitro

```js
import { modular, context, config } from 'http-modular';

function add(x, y) {
  return x + y;
}

const echo = context(async (ctx) => {
  return await readBody(ctx);
})

function getMessage() {
  return {hi: 'there'};
}

export default eventHandler(
  modular({add, echo, getMessage}, config.nitro)
);
```

### 5. Integrating with Vercel API Functions:

```js
import { modular, context, config } from 'http-modular';

function add(x, y) {
  return x + y;
}

const echo = context(ctx => ctx.request.body);

function getMessage() {
  return {hi: 'there'};
}

export default modular({add, echo, getMessage}, config.vercel);
```

### 6. Integrating with AirCode Cloud Functions:

```js
import {config, context, modular} from 'http-modular';

function add(x, y) {
  return x + y;
}

const getUrl = context(ctx => ctx.url);

export default modular({
  add,
  getUrl,
}, config.aircode);
```
