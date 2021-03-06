<img src="https://raw.githubusercontent.com/zeit/art/b1c55af9827fc0293bc5a9c1a03e37e08a6f8f16/micro-dev/logo.png" width="80"/>

This command line interface provides a belt full of tools that make building microservices using [micro](https://github.com/zeit/micro) a breeze! It's only meant to be used in development, **not in production** (that's where [micro](https://github.com/zeit/micro) comes in).

[![Build Status](https://travis-ci.org/zeit/micro-dev.svg?branch=master)](https://travis-ci.org/zeit/micro-dev)
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/micro)
[![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/sindresorhus/xo)

## Features

- **Hot Reloading:** When making changes to your code, the server will restart by itself
- **Logs:** Incoming and outgoing requests will be logged to `stdout`
- **Beautiful JSON:** When JSON bodies are logged, they're styled and prettified
- **Clipboard Support:** The local address is pasted to the clipboard automatically
- **Port Selection:** Automatic detection and use of an open port (if the specified one is in use)
- **Debug in Your Network:** The network address shown in addition to local one
- **Duration Logs:** Calculates and shows the duration for each request
- **Pretty Errors:** Prettifies the `Error` object if any exceptions are thrown

## Usage

**Important:** This tool is only meant to be used in development. In production, you should use [micro](https://github.com/zeit/micro), which is much lighter and faster (and also comes without the belt of tools used when developing microservices).

When preparing your development environment, firstly install `micro-dev`:

```bash
npm install --save-dev micro-dev
```

**Note:** You'll need at least Node.js v7.6.0 to run `micro-dev`.

Next, add a new `script` property below `micro` inside `package.json`:

```json
"scripts": {
  "start": "micro",
  "dev": "micro-dev"
}
```

As the final step, start the development server like this:

```bash
npm run dev
```

## Programmatic Usage

Unfortunately, due to the nature of hot reload, server states are different across each restart. This is a big problem for using middlewares (e.g. socket.io, Apollo or such) as we cannot just return the created server object.

Therefore, you will need a notification callback which is fired before each server start/restart, and inside that callback, you will associate/re-associate middlewares.

This should be the easiest workaround right now. Now you could just pass your callback to the last argument.

```js
micro(file, flags, cbBeforeStart)
```

If you are using the code from [this issue comment](https://github.com/zeit/micro/issues/337#issuecomment-365670943), just change to the following code:
```js
const micro = require('micro-dev')
micro(handlerPath, flags, (server) => {
  console.log('server is going to start!')
})
```

*Caveat 1*: **The server is managed by micro-dev after the `micro` call, do not add any user code after it!**

*Caveat 2*: **Code inside the callback isn't likely to be candidated for hot reload watching, unless you modularized your callback (i.e. it's part of the requrie chain).** Please do not naively require the callback file, but instead, wrap it with a lambda.

## Contributing

1. [Fork](https://help.github.com/articles/fork-a-repo/) this repository to your own GitHub account and then [clone](https://help.github.com/articles/cloning-a-repository/) it to your local device
2. Move into the directory of the clone: `cd micro-dev`
3. Link it to the global module directory of Node.js: `npm link`

Inside the project where you want to test your clone of the package, you can now either use `npm link micro-dev` to link the cloned source to your project's local dependencies or run `micro-dev` right in your terminal.

## Authors

- Leo Lamprecht ([@notquiteleo](https://twitter.com/notquiteleo)) - [▲ZEIT](https://zeit.co)
- Tim Neutkens ([@timneutkens](https://twitter.com/timneutkens))
