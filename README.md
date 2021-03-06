# gulp-srcset

[![NPM version][npm]][npm-url]
[![Node version][node]][node-url]
[![Dependency status][deps]][deps-url]

[npm]: https://img.shields.io/npm/v/gulp-srcset.svg
[npm-url]: https://www.npmjs.com/package/gulp-srcset

[node]: https://img.shields.io/node/v/gulp-srcset.svg
[node-url]: https://nodejs.org

[deps]: https://img.shields.io/david/TrigenSoftware/gulp-srcset.svg
[deps-url]: https://david-dm.org/TrigenSoftware/gulp-srcset

Highly customizable plugin to generating responsive images.

# Install

```bash
npm i -D gulp-srcset
# or
yarn add -D gulp-srcset
```

# API

## `gulpSrcsetPlugin(rules [, options])`

### `Array<Object rule> rules`

Array of rules to generate variants of image.

#### `String|Function|Array<String|Function> rule.match`

There is support of 3 types of matchers:

1. Glob pattern of file path/name.
2. Media query to match image by size.
3. Function with `path`, `size` and `file` arguments, where `path` is String, `size` is object `{ width, height }` and `file` is instance of `Vinyl`.

#### `Number|Array<Number> rule.width`

Target widths to generate, value less or equal 1 will be detected as multiplier.

`gulp-srsset` supports SVG, GIF, JPEG, PNG and WebP, but only last 3 formats available to resize.

Default: `[1]`

#### `String|Array<String> rule.format`

Target formats to generate, supports: `"svg"`, `"gif"`, `"jpeg"`, `"png"` and `"webp"`.

For converting are available only `"jpeg"`, `"png"` and `"webp"`.

Default: ```[`format of source image`]```

#### `String rule.postfix(Number calculatedWidth, Number width)`

Function to generate postfix for file name.

Default: ```(calculatedWidth, width) => width == 1 ? '' : `@${calculatedWidth}w` ```

#### `Object rule.processing`

Object with [Sharp configs](http://sharp.readthedocs.io/en/stable/api-output/) for each supported format. Sharp used as tool for resizing and converting images.

Default:
```js
{
	webp: {
		quality: 100
	},
	jpg: {
		quality: 100
	},
	png: {}
}
```

#### `Object rule.optimization`

Object with [imagemin](https://www.npmjs.com/package/imagemin) plugins for each format. Imagemin used as tool for images optimization.

Default:
```js
{
	webp: webpPlugin({
		quality: 100
	}),
	jpg:  mozJpegPlugin({
		quality: 100
	}),
	png:  zopfliPlugin(),
	gif:  gifLossyPlugin(),
	svg:  svgoPlugin()
}
```

#### `Boolean rule.skipOptimization`

Option to skip optimization.

Default: `false`

#### `Boolean rule.scalingUp`

Generate or not images with higher width than they's sources are.

Default: `true`

### `Object options`

Object with common config.

#### `String options.postfix(Number calculatedWidth, Number width)`

Function to generate postfix for file name.

Default: ```(calculatedWidth, width) => width == 1 ? '' : `@${calculatedWidth}w` ```

#### `Object options.processing`

Object with [Sharp configs](http://sharp.readthedocs.io/en/stable/api-output/) for each supported format. Sharp used as tool for resizing and converting images.

Default:
```js
{
	webp: {
		quality: 100
	},
	jpg: {
		quality: 100
	},
	png: {}
}
```

#### `Object options.optimization`

Object with [imagemin](https://www.npmjs.com/package/imagemin) plugins for each format. Imagemin used as tool for images optimization.

Default:
```js
{
	webp: webpPlugin({
		quality: 100
	}),
	jpg:  mozJpegPlugin({
		quality: 100
	}),
	png:  zopfliPlugin(),
	gif:  gifLossyPlugin(),
	svg:  svgoPlugin()
}
```

#### `Boolean options.skipOptimization`

Option to skip optimization.

Default: `false`

#### `Boolean options.scalingUp`

Generate or not images with higher width than they's sources are.

Default: `true`

# Example 
[`gulpfile.js`](https://github.com/TrigenSoftware/gulp-srcset/tree/master/example)
```js
const gulp = require('gulp'),
	srcset = require('gulp-srcset');

gulp.task('images', () =>
	gulp.src('src/*.{jpg,png,gif}')
		.pipe(srcset([{
			match:  '(min-width: 3000px)',
			width:  [1, 1920, 1280, 720, 560, 320],
			format: ['jpg', 'webp']
		}]))
		.pipe(gulp.dest('dist'))
);
```

---
[![NPM](https://nodei.co/npm/gulp-srcset.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/gulp-srcset/)
