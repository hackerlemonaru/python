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
pip install restless-sdk
```

## Use

```python
import os
import restless

client = restless.Restless(os.environ["RESTLESS_KEY"])

@client.setup
def _(request):
    return {
        "api_key": client.mask(request.headers.get("authorization")),
        "owner": {
            # Permanent and immutable. A workspace id or database primary
            # key, never an API key, email, or anything else that rotates:
            # the dashboard pins a project's whole log history to this.
            "id": workspace_id_for(request),
            # Runs once per owner id, then caches. Put the expensive lookup
            # here, not in the fields above.
            "enrich": lambda owner_id: {
                "label": db.workspaces.get(owner_id).name,
                "email": db.workspaces.get(owner_id).admin_emails,
            },
        },
    }
```

Then wrap your app:

```python
# Flask / Django / any WSGI
app.wsgi_app = client.wsgi(app.wsgi_app)

