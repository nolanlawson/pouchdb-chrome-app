# pouchdb-chrome-app

PouchDB runs great in Chrome packaged apps. Here's how to get started.

Sample app
-----

* [PouchDB "hello world" Chrome app](https://github.com/nolanlawson/pouchdb-chrome-app-hello-world)

Installation
-------

To use PouchDB in your Chrome app, just download [pouchdb.js](http://pouchdb.com/guides/setup-pouchdb.html) and include it in your `index.html`:

```html
<script src="path/to/pouchdb.js"></script>
```

Now `PouchDB` is available as a global variable. So you can create a new PouchDB:

```js
var db = new PouchDB('mydb');
```

Note that only the IndexedDB adapter (i.e. the default adapter) is supported, because Chrome apps disallow WebSQL.

Using `query()`
-----

By default, the `query()` API will not work in Chrome apps, because PouchDB needs to use `eval()`, which is disallowed by Chrome apps' Content Security Policy (CSP).

You could modify the CSP, e.g. by adding `"content_security_policy": "script-src 'self' 'unsafe-eval'; object-src 'self'"` to the `manifest.json`, but then your app has to run [in a sandbox](http://blog.chromium.org/2008/10/new-approach-to-browser-security-google.html), which is probably not what you want.

To work around this problem, you have a few options:

* Use the [pouchdb-find plugin](https://github.com/nolanlawson/pouchdb-find), which is a newer API designed to replace `query()`, and which does not use `eval()`.
* Use the [noeval plugin](https://github.com/evidenceprime/pouchdb.mapreduce.noeval), which tries to mimic as much of the `query()` API as possible without using `eval()`.

Both plugins can be installed by simply adding the script tag to your `index.html`. E.g. for `pouchdb-find`:

```html
<script src="path/to/pouchdb.js"></script>
<script src="path/to/pouchdb.find.js"></script>
```

Another option is to simply not use the `query()` API, and to use `allDocs()` instead. Instructions for this technique can be found in the [PouchDB pro tips](http://pouchdb.com/2014/06/17/12-pro-tips-for-better-code-with-pouchdb.html) (tip #7).

Using filtered replication
------

With [filtered replication](http://pouchdb.com/api.html#filtered-replication), filter functions inside of design documents are not supported, again because of the `eval()` limitation. Luckily you can still use `doc_ids`, ad-hoc filter functions, and server-side filtering.
