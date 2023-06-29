# Welcome to YUIDoc {{version}}!

YUIDoc is a <a href="http://nodejs.org/">Node.js</a> application that generates API documentation from comments in source, using a syntax similar to tools like Javadoc and Doxygen. YUIDoc provides:

- **Live previews.** YUIDoc includes a <a href="args/index.html#server">standalone doc server</a>, making it trivial to preview your docs as you write.

- **Modern markup.** YUIDoc's generated documentation is an <a href="http://yuilibrary.com/yui/docs/api/classes/Model.html">attractive, functional web application</a> with real URLs and graceful fallbacks for spiders and other agents that can't run JavaScript.

- **Wide language support.** YUIDoc was originally designed for the <a href="http://yuilibrary.com">YUI project</a>, but it is not tied to any particular library or programming language. You can use it with any language that supports `/* */` comment blocks.

## Installation and Usage

1. Download and install <a href="http://nodejs.org/#download">Node.js</a>
2. Run `npm -g install yuidocjs`.
3. Run `yuidoc .` at the top of your JS source tree.

That's it! For more information about running the `yuidoc` commandline tool, refer to "<a href="args/index.html">Using YUIDoc</a>".

## User Guides

- [Using YUIDoc]() — Understanding YUIDoc command line arguments and usage.
- [YUIDoc Syntax Reference]() — Detailed instructions for writing YUIDoc comment blocks.
- [YUIDoc Themes]() — How to modify the default YUIDoc theme.


## Example YUIDoc Comment Blocks

YUIDoc parses a modified form of JSDoc tags. This section provides a taste of some of the more common constructs in YUIDoc. For more information, refer to the "<a href="./syntax/index.html">YUIDoc Syntax Reference</a>".

### Example Class Block

```
/**
* This is the description for my class.
*
* @class MyClass
* @constructor
*/
```
### Example Method Block

```
/**
* My method description.  Like other pieces of your comment blocks, 
* this can span multiple lines.
*
* @method methodName
* @param {String} foo Argument 1
* @param {Object} config A config object
* @param {String} config.name The name on the config object
* @param {Function} config.callback A callback function on the config object
* @param {Boolean} [extra=false] Do extra, optional work
* @return {Boolean} Returns true on success
*/
```

### Example Property Block

```
/**
* My property description.  Like other pieces of your comment blocks, 
* this can span multiple lines.
* 
* @property propertyName
* @type {Object}
* @default "foo"
*/
```
