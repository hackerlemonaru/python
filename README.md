<a href="https://restless.ai">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="docs/restless-init-dark.svg">
    <img width="100%" src="docs/restless-init.svg" alt="Restless">
  </picture>
</a>

Run in your codebase to get started:

```sh
npx restless init
```

This scans your project, figures out your framework, generates an OpenAPI spec, and automatically wires the SDK into your server.

# restless-sdk

Capture your API traffic and send it to [Restless](https://restless.ai).

Works with any **WSGI** app (Flask, Django, Pyramid, Bottle) or **ASGI** app
(FastAPI, Starlette, Quart). Python 3.8+. No dependencies.

## Manual installation

If you'd rather wire it up by hand:

```sh
