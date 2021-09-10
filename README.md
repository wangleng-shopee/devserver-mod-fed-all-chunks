# Basic One-Way Example

This example demos a basic host application loading remote component.

- `app1` is the host application.
- `app2` standalone application which exposes `Button` component.

# Running Demo

Run `yarn start`. This will build and serve both `app1` and `app2` on ports 3001 and 3002 respectively.

- [localhost:3001](http://localhost:3001/) (HOST)
- [localhost:3002](http://localhost:3002/) (STANDALONE REMOTE)

<img src="https://ssl.google-analytics.com/collect?v=1&t=event&ec=email&ea=open&t=event&tid=UA-120967034-1&z=1589682154&cid=ae045149-9d17-0367-bbb0-11c41d92b411&dt=ModuleFederationExamples&dp=/email/BasicRemoteHost">

## Issue Demonstrated

If you visit http://localhost:3001 on your browser, you see that it fails to load
the button from http://localhost:3002.

This is because the `remoteEntry.js` from http://localhost:3002 exports an empty
object for `window.app2`, as the chunk that `remoteEntry.js` depends on is missing.

Normally, chunk loading is handled by `HtmlWebpackPlugin` injecting the vendor chunks
to the head, so this is not an issue for html-based entry. However, if the entry
is from `remoteEntry.js` (the module federation way), unless we find some way to inject
the vendor chunks ourselves, it will not load correctly.

Other notes:
* Does not happen in production. Only when webpack-dev-server is involved does the chunk
  splitting suddenly become an issue.
* Does not happen if you remove `splitChunks: { chunks: 'all' }` and use the default (or
  disable chunks completely). This is a workaround.
