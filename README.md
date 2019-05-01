# ionic-bower

Bower repository for [Ionic Framework](http://github.com/driftyco/ionic) v1 (Ionic 2+ uses npm)

### Usage

Include `js/ionic.bundle.js` to get ionic and all of its dependencies.

Alternatively, include the individual ionic files with the dependencies separately.

### Versions

To install the latest stable version, `bower install driftyco/ionic-bower`

To install the latest nightly release, `bower install driftyco/ionic-bower#master`

### angular.equals patch (CRITICAL!)

There's an issue with angular attempting to compare `$jsonapi` records resulting in infinite recursion.  To fix, the
following section in `bower_components/ionic/js/ionic.bundle.js`

```js
function equals(o1, o2) {
  if (o1 === o2) return true;
  if (o1 === null || o2 === null) return false;
  if (o1 !== o1 && o2 !== o2) return true; // NaN === NaN
  var t1 = typeof o1, t2 = typeof o2, length, key, keySet;
  if (t1 == t2 && t1 == 'object') {
```
with

```js
function equals(o1, o2) {
  if (o1 === o2) return true;
  if (o1 === null || o2 === null) return false;
  if (o1 !== o1 && o2 !== o2) return true; // NaN === NaN
  var t1 = typeof o1, t2 = typeof o2, length, key, keySet;
  if (t1 == t2 && t1 == 'object') {

    // Patch to fix comparison of similar records
    if (o1.__type && o1.__type === o2.__type && o1.id && o1.id === o2.id) {
      return true;
    }
    // End patch
...
```

and rebuild with `uglify-js` using the following command:

```sh
uglifyjs js/ionic.bundle.js -o js/ionic.bundle.min.js
```
