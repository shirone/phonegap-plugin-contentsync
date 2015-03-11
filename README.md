#phonegap-plugin-contentsync [![Build Status][travis-ci-img]][travis-ci-url]

> Download and cache remotely hosted content.

_This plugin is a work in progress and it is not production ready._

## Installation

`phonegap plugin add https://github.com/phonegap/phonegap-plugin-contentsync`

## Supported Platforms

- Android
- iOS
- WP8


## Quick Example

```javascript
var sync = ContentSync.sync({ src: 'http://myserver/assets/movie-1', id: 'movie-1' });

sync.on('progress', function(data) {
    // data.progress
});

sync.on('complete', function(data) {
    // data.localPath
});

sync.on('error', function(e) {
    // e.message
});

sync.on('cancel', function() {
    // triggered if event is cancelled
});
```

## API

### ContentSync.sync(options)

Parameter | Description
--------- | ------------
`options.src` | `String` URL to the remotely hosted content.
`options.id` | `String` Unique identifer to reference the cached content.
`options.type` | `String` _(Optional)_ Defines the copy strategy for the cached content.<br/>The type `replace` is the default behaviour that deletes the old content and caches the new content.<br/> The type `merge` will add the new content to the existing content. This will replace existing files, add new files, but never delete files.
`options.headers` | `Object` _(Optional)_ Set of headers to use when requesting the remote content from `options.src`.

#### Returns

- Instance of `ContentSync`.

#### Example

```javascript
var sync = ContentSync.sync({ src: 'http://myserver/app/1', id: 'app-1' });
```

### sync.on(event, callback)

Parameter | Description
--------- | ------------
`event` | `String` Name of the event to listen to.<br/>The event `progress` will trigger on each update as the native platform downloads and caches the content.<br/>The event `complete` will trigger when the content has been successfully cached onto the device.<br/>The event `error` will trigger when an internal error occurs and the cache is aborted.<br/>The event `cancel` will trigger when `sync.cancel` is called.
`callback` | `Function` is called when the event is triggered.

#### Event: `progress`

Callback Parameter | Description
------------------ | -----------
`data.progress` | `Integer` Progress percentage between `0 - 100`. The progress includes all actions required to cache the remote content locally. This is different on each platform, but often includes requesting, downloading, and extracting the cached content along with any system cleanup tasks.
`data.status` | `String` Briefly describes the current task status, such as "Downloading" or "Extracting"

#### Event: `complete`

Callback Parameter | Description
------------------ | -----------
`data.localPath` | `String` The file path to the cached content. The file path will be different on each platform and may be relative or absolute. However, it is guaraneteed to be a compatible reference in the browser.

#### Event: `error`

Callback Parameter | Description
------------------ | -----------
`e` | `Error` Standard JavaScript error object that describes the error.

#### Event: `cancel`

Callback Parameter | Description
------------------ | -----------
`no parameters` |

#### sync.cancel()

Cancels the content sync operation and fires the cancel callback.

```
var sync = ContentSync.sync({ src: 'http://myserver/app/1', id: 'app-1' });

sync.on('cancel', function() {
    console.log('content sync was cancelled');
});

sync.cancel();
```

## Native Requirements

- There should be no dependency on the existing File or FileTransfer plugins.
- The native cached file path should be uniquely identifiable with the `id` parameter. This will allow the Content Sync plugin to lookup the file path at a later time using the `id` parameter.
- The first version of the plugin assumes that all cached content is downloaded as a compressed ZIP. The native implementation must properly extract content and clean up any temporary files, such as the downloaded zip.
- The locally compiled Cordova web assets should be copied to the cached content. This includes `cordova.js`, `cordova_plugins.js`, and `plugins/**/*`.
- Multiple syncs should be supported at the same time.

## Running Tests

    $ npm test

## Contributing

### Editor Config

The project uses [.editorconfig](http://editorconfig.org/) to define the coding
style of each file. We recommend that you install the Editor Config extension
for your preferred IDE.

### JSHint

The project uses [.jshint](http://jshint.com/docs) to define the JavaScript
coding conventions. Most editors now have a JSHint add-on to provide on-save
or on-edit linting.

#### Install JSHint for vim

1. Install [jshint](https://www.npmjs.com/package/jshint).
1. Install [jshint.vim](https://github.com/wookiehangover/jshint.vim).

#### Install JSHint for Sublime

1. Install [Package Control](https://packagecontrol.io/installation)
1. Restart Sublime
1. Type `CMD+SHIFT+P`
1. Type _Install Package_
1. Type _JSHint Gutter_
1. Sublime -> Preferences -> Package Settings -> JSHint Gutter
1. Set `lint_on_load` and `lint_on_save` to `true`

[travis-ci-img]: https://travis-ci.org/phonegap/phonegap-plugin-contentsync.svg?branch=master
[travis-ci-url]: http://travis-ci.org/phonegap/phonegap-plugin-contentsync
