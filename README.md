# angular-imgcache.js

Simple [imgcache.js](https://github.com/chrisben/imgcache.js) wrapper for AngularJS, can be used with Ionic/Cordova/Phonegap.

## Installation

Install via npm
```sh
npm install --save imgcache.js angular-imgcache.js
```

Install via Yarn
```sh
yarn add imgcache.js angular-imgcache.js
```

Install via bower (deprecated, use npm or yarn instead)
```sh
bower install --save angular-imgcache.js
```


There is only a single file that you need to include: `angular-imgcache.js`

Example html
```html
<script src="node_modules/angular/angular.js"></script>
<script src="node_modules/imgcache.js/js/imgcache.js"></script>

<!-- Add this: -->
<script src="node_modules/angular-imgcache.js/angular-imgcache.js"></script>
```

### Load module

```javascript
angular.module('MyApp', [
    'ImgCache'
])
```

## Usage

### Configuration

You can override imgcache.js default options in Angulars config section.

```javascript
.config(function(..., ImgCacheProvider) {

    // set single options
    ImgCacheProvider.setOption('debug', true);
    ImgCacheProvider.setOption('usePersistentCache', true);

    // or more options at once
    ImgCacheProvider.setOptions({
        debug: true,
        usePersistentCache: true
    });

    // ImgCache library is initialized automatically,
    // but set this option if you are using platform like Ionic -
    // in this case we need init imgcache.js manually after device is ready
    ImgCacheProvider.manualInit = true;

    // If you want to, you can disable the cache.
    // This might be useful to disable it to get rid of CORS issues when
    // developing cordova Applications using a browser.
    ImgCacheProvider.disableCache(true);

    ...

});
```

If you are using platform like Ionic, you have to init ImgCache manually. Note: you must set `ImgCacheProvider.manualInit = true;` as in example above.

```javascript
.run(function($ionicPlatform, ImgCache) {

    $ionicPlatform.ready(function() {
        ImgCache.$init();
    });

});
```

You can init library manually in other patforms via "deviceready" callback like this:

```html
<body onload="onLoad()">
```

```javascript
function onLoad() {
    document.addEventListener("deviceready", onDeviceReady, false);
}

function onDeviceReady() {
    ImgCache.$init();
}
```

(Thx @emps for this note in #1)

### Service

Access imgcache.js and its original methods in your components via promise to make sure that imgcache.js library is already initialized

```javascript
.controller('MyCtrl', function($scope, ImgCache) {

    ImgCache.$promise.then(function() {
        ImgCache.cacheFile('...');
    });

});
```

### Directive

Angular-imgcache.js comes with directive, which first looks into cache for an image. If not present, it downloads the image, then stores in cache and uses it.

We can set src of an image with `ic-src` attribute.

```html
<img img-cache ic-src="{{imgUrl}}" />
```

Or set elements `background-image` with `ic-bg` attribute.

```html
<div img-cache ic-bg="{{imgUrl}}" ></div>
```

## Contributing

If you have any issues, please feel free to open an issue.

If you want to contribute code, feel free to do that as well! I will be extra happy if you also check you code using [jscs](http://jscs.info/) and [jshint](http://jshint.com/)
