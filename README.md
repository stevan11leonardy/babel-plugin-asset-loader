# babel-plugin-file-loader [![CircleCI](https://circleci.com/gh/sheerun/babel-plugin-file-loader/tree/master.svg?style=svg)](https://circleci.com/gh/sheerun/babel-plugin-file-loader/tree/master) [![npm][npm-badge]][npm-link]

## Installation

```
npm install babel-plugin-asset-loader --save
```

```
yarn add babel-plugin-asset-loader
```

Then put following "asset-loader" as plugin in .babelrc:

```json
{
  "plugins": ["asset-loader"]
}
```

This is equivalent to following default configuration:

```json
{
  "plugins": [
    [
      "asset-loader",
      {
        "name": "[hash].[ext]",
        "extensions": ["png", "jpg", "jpeg", "gif", "svg"],
        "outputPath": "/public",
        "context": "",
        "limit": 0
      }
    ]
  ]
}
```

## Feature
This will import your asset into your package using babel and feel free to use your asset.

## How it works

More or less as follows:

1. Processes only `import` and `require` that reference files ending with one of `"extensions"`
2. Calculates actual `$name` of resource by substituting placeholders in `"name"`
3. Copies resource into `$ROOT/$outputPath/$name` where `$ROOT` is `.babelrc` location.
3. Replaces `import` and `require` in code with `"$publicPath/$name"` string



## Options

### outputPath

Tells where to put static files. By default it's `"/public"`.

This path is relative to the root of project. Setting value `null` prevents the plugin to copy the file.

### name

The default is `[hash].[ext]` where:

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|**`[ext]`**|`{String}`|`file.extname`|The extension of the resource|
|**`[name]`**|`{String}`|`file.basename`|The basename of the resource|
|**`[path]`**|`{String}`|`file.dirname`|The path of the resource relative to the `context`|
|**`[hash]`**|`{String}`|`md5`|The hash of the content, see below for more info|

The full format `[hash]` is: `[<hashType>:hash:<digestType>:<length>]`, where:

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|**`hashType`**|`{String}`|`md5`|`sha1`, `md5`, `sha256`, `sha512`|
|**`digestType`**|`{String}`|`base64`|`hex`, `base26`, `base32`, `base36`, `base49`, `base52`, `base58`, `base62`, `base64`|
|**`length`**|`{Number}`|`128`|The length in chars|

For example: `[md5:hash:base58:8]` or `[hash:base36]`.

### extensions

List of extension file-loader should look for in imports. All other imports are ignored.

### context

Path to directory relative to `.babelrc` where application source resides. By default `""`, but can be e.g. `"/src"`.

### limit

Value in byte to determine if the content is base64 inlined. In that case, the file is not copy to `outputPath`. It replicates [url-loader](https://github.com/webpack-contrib/url-loader) webpack loader behaviour.

Default is 0 which means nothing is inlined.

## Contributing

Yes, please!

## License

[MIT](./LICENSE)

[npm-badge]: https://img.shields.io/npm/v/babel-plugin-file-loader.svg?style=flat-square
[npm-link]: https://www.npmjs.com/package/babel-plugin-file-loader
