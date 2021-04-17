---
layout: layouts/post.njk
title: Deploying a SvelteKit application to Heroku
date: 2021-04-17T17:23:49.588Z
tags:
  - meta
---
I've been a fan of [Svelte](https://svelte.dev) for a few years now, so I'm very excited about [SvelteKit](https://kit.svelte.dev/).

I've begun migrating an existing Node + Express project that's hosted on Heroku to SvelteKit, and I couldn't find any clear instructions about how to make the deployment work using SvelteKit's [Node adapter](https://github.com/sveltejs/kit/tree/master/packages/adapter-node).

At first I encountered an error about `svelte-kit` not being found. This turned out to be Heroku helpfully pruning `devDependencies`, which removes `@sveltejs/kit` from the built application. I [disabled this behaviour](https://devcenter.heroku.com/articles/nodejs-support#skip-pruning) by setting the `NPM_CONFIG_PRODUCTION` variable to `false`:

```bash
$ heroku config:set NPM_CONFIG_PRODUCTION=false
```

Then, while the app deployed successfully, it kept crashing with an [R10 error](https://help.heroku.com/P1AVPANS/why-is-my-node-js-app-crashing-with-an-r10-error) shortly after launch. It turns out that the `Procfile` must be set to the following (assuming that your `start` script runs `svelte-kit start`):

```
web: npm run start -- --port $PORT --host 0.0.0.0
```

I hope this is helpful for anyone else trying to achieve the same thing.