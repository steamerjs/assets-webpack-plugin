assets-webpack-plugin
=====================

Webpack plugin that emits a json file with assets paths.

## Why is this useful?

When working with Webpack will probably want to generate you bundles with a generate hash in them (for cache busting).

This plug-in generates a json file with the generated assets to be used somewhere else, I use this with Rails so it can find the assets.

## Install

```
npm install assets-webpack-plugin --save
```

## Configuration

In you webpack config include the plug-in. And add it to your config:

```
var path = require("path");
var SaveAssetsJson = require('assets-webpack-plugin');

module.exports = {
  ...
  output: {
    path: path.join(__dirname, "public", "js"),
    filename: "[name]-bundle-[hash].js",
    publicPath: "/js/"
  },
  ....
  plugins: [new SaveAssetsJson()]
};  
```

### Options

You can pass the following options:

__path__: 

Path where to save the created json file. Defaults to the current directory.

```
new SaveHashes({path: path.join(__dirname, 'app', 'views')})
```

__filename__: 

Name for the created json file. Defaults to `webpack-assets.json`

```
new SaveHashes({filename: 'assets.json'})
```

### Using this with Rails

I use this with Rails, so it can find my Webpack assets. In my application controller I have:

```ruby
def script_for(bundle)
  path = Rails.root.join('app', 'views', 'webpack-assets.json') # This is the file generated by the plug-in
  file = File.read(path)
  json = JSON.parse(file)
  json[bundle]
end
```

Then in the actions:

```ruby
def show
  @script = script_for('clients') # this will retrive the bundle named 'clients'
end
```

And finally in the views:

```
<div id="app">
  <script src="<%= @script %>"></script>
</div>
```

