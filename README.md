# Angular Template loader for webpack

Puts HTML partials in the Angular's $templateCache so directives can use templates without initial downloading.

## Webpack and loaders

Webpack is the [webpack](http://webpack.github.io/) and it's module bundler. [Loaders](http://webpack.github.io/docs/using-loaders.html) wrap content in the javascript code that executes in the browser. 

# Install

`npm install ng-cache-loader`

# Usage

You can require html partials via `ng-cache-loader`:

``` javascript
require('ng-cache!./demo/template/myPartial.html');
```

Partial will be available as `ng-include="'myPartial.html'"`
or `templateUrl: 'myPartial.html'`.

## Named templates

You can wrap template in the `script` tag:

``` html
<!-- ./demo/template/myPartial.html -->

<script type ="text/ng-template" id="myFirstTemplate">
  <!-- then use ng-include="'myFirstTemplate'" -->
</script>
```

You can have multiple templates in one file:

``` html
<!-- ./demo/template/myPartial.html -->

<script type ="text/ng-template" id="myFirstTemplate">
  <!-- then use ng-include="'myFirstTemplate'" -->
</script>

<script type ="text/ng-template" id="mySecondTemplate">
  <!-- then use ng-include="'mySecondTemplate'" -->
</script>
```

You can mix named templates and simple markup:

``` html
<!-- ./demo/template/myPartial.html -->

<script type ="text/ng-template" id="myFirstTemplate">
  <!-- then use ng-include="'myFirstTemplate'" -->
</script>

<!-- markup outside script tags available as ng-include="'myPartial.html'" -->
<div>...</div>

<script type ="text/ng-template" id="mySecondTemplate">
  <!-- then use ng-include="'mySecondTemplate'" -->
</script>
```

## Prefix

Prefix adds path left of template name:

``` javascript
require('ng-cache?prefix=public/templates!./path/to/myPartial.html')
// => ng-include="'public/templates/myPartial.html'"
```

Prefix can mask the real directory with the explicit value
or retrieve the real directory name (use `[dir]`):

``` javascript
require('ng-cache?prefix=public/[dir]/templates!./path/to/myPartial.html')
// => ng-include="'public/path/templates/myPartial.html'" 
```

Prefix can strip the real directory name (use `//`):

``` javascript
require('ng-cache?prefix=public/[dir]//[dir]/templates!./far/far/away/path/to/myPartial.html')
// => ng-include="'public/far/path/templates/myPartial.html'" 
```

## webpack config

Match `.html` extension with loader:

``` javascript
module: {
    loaders: [
        {
            test: /.html$/,
            loader: "ng-cache?prefix=[dir]/[dir]"
        }
    ]
},
```

# License

MIT (http://www.opensource.org/licenses/mit-license.php)