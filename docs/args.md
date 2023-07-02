# Args 

Generating documentation with YUIDoc is as simple as going to the top of your source tree and running:
```terminal
$ yuidoc .
```
However, you can configure YUIDoc's behavior further by providing <a href="#running-yuidoc-on-the-command-line">command line options</a>, a <a href="#configuring-yuidoc-with-yuidocjson">JSON configuration file</a>, or even both.

This section discusses the `yuidoc` command line tool in more detail.

## Running YUIDoc on the Command Line

Certain `yuidoc` command line options correspond to entries in the  
the <a href="#configuring-yuidoc-with-yuidocjson">`yuidoc.json` configuration file</a>.
Command line options always take priority.

```terminal
YUI Doc generates API documentation from a modified JavaDoc syntax.

Current version (0.10.0)

Usage: yuidoc <options> <input path>

Common Options:
  -c, --config, --configfile <filename>  A JSON config file to provide configuration data.
           You can also create a yuidoc.json file and place it
           anywhere under your source tree and YUI Doc will find it
           and use it.
  -e, --extension <comma sep list of file extensions> The list of file extensions to parse 
           for api documentation. (defaults to .js)
  -x, --exclude <comma sep list of directories> Directories to exclude from parsing 
           (defaults to '.DS_Store,.svn,CVS,.git,build_rollup_tmp,build_tmp')
  -v, --version Show the current YUIDoc version
  --project-version Set the doc version for the template
  -N, --no-color Turn off terminal colors (for automation)
  -C, --no-code Turn off code generation (don't include source files in output)
  -n, --norecurse Do not recurse directories (default is to recurse)
  --no-sort Do not alphabetical sorting of attributes, events, methods, and properties
  -S, --selleck Look for Selleck component data and attach to API meta data
  -V, --view Dump the Handlebars.js view data instead of writing template files
  -p, --parse-only Only parse the API docs and create the JSON data, do not render templates
  -o, --outdir <directory path> Path to put the generated files (defaults to ./out)
  -t, --themedir <directory path> Path to a custom theme directory containing Handlebars templates
  -H, --helpers <comma separated list of paths to files> Require these file and add Handlebars helpers. See docs for more information
  --charset CHARSET Use this as the default charset for all file operations. Defaults to 'utf8'
  -h, --help Show this help
  -q, --quiet Supress logging output
  -T, --theme <simple|default> Choose one of the built in themes (default is default)
  --syntaxtype <js|coffee> Choose comment syntax type (default is js)
  --server <port> Fire up the YUIDoc server for faster API doc developement. Pass optional port to listen on. (default is 3000)
  --lint Lint your docs, will print parser warnings and exit code 1 if there are any

  <input path> Supply a list of paths (shell globbing is handy here)
```

### Running YUIDoc in Server Mode

Most documentation tools (including YUIDoc) involve some sort of build process.
However, YUIDoc provides a unique feature that allows you to short-circuit this.

In <dfn>server mode</dfn>, 
YUIDoc fires up a small Node.js based server and 
begins parsing and displaying documentation in real time.
This greatly speeds up the documentation authoring process,
as you can edit your source code and 
preview changes with a simple browser reload,
rather than waiting for a build.

To activate server mode on `localhost:3000`, run `yuidoc --server`.
You can optionally specify an alternate port:

```terminal
    yuidoc --server
        or
    yuidoc --server 5000
```

Then visit:

```terminal
    http://127.0.0.1:3000/
        or
    http://127.0.0.1:5000/
```

Any changes you make to your YUIDoc comment blocks. 
will be reflected when you reload the browser. 
It's that simple!

<strong>NOTE:</strong> Server mode is not
a replacement for building and hosting your documentation,
just a handy previewing tool. 
For production, you should generate static HTML pages and 
host them on a real web server.

### Working with YUIDoc Parsed Data

YUIDoc generates a `data.json` file after it parses your API documentation. 
The `external.data` config option enables you to import a YUIDoc `data.json` file 
from another project and mix it into your own documentation. 

This feature is handy when you are extending another project and would like to link back
to their API documentation. For example, importing YUI Library's `data.json`
file would enable YUIDoc to automatically link back to `Base`, `EventTarget`, and
other core YUI objects that your own API might be extending or mixing in. 

Currently, importing external data enables YUIDoc to resolve HTML links `@extends` or `@use` keywords, 
but does <em>not</em> cause YUIDoc to generate complete documentation for the external API.
Future versions of YUIDoc may provide the option to mix in the data natively 
and reproduce the external API right along with your own.

#### Adding External YUIDoc Data to Your Project

Create an `external` object under the `options` object in your `yuidoc.json` file and give it
a property called `data` pointing to the URL of the external `data.json` file you wish to import. 
`data` can be a string or an array of strings. 

```
{
  "options": {
    "external": {
      "data": "http://yuilibrary.com/yui/docs/api/data.json"
    }
  }
}
```

Also, you are able to give `base` data within `external` for external base URLs.

```
{
  "options": {
    "external": {
      "data": [
        {
          "base": "http://emberjs.com/api/",
          "json": "http://builds.emberjs.com/tags/v1.5.1/ember-docs.json"
        },
        {
          "base": "http://emberjs.com/api/",
          "json": "http://builds.emberjs.com/tags/v1.0.0-beta.6/ember-data-docs.json"
        }
      ]
    }
  }
}
```

<strong>NOTE:</strong> YUIDoc currently fetches external data on each run with no caching. 

## Configuring YUIDoc with yuidoc.json

You can also store most YUIDoc configurables in a `yuidoc.json` file.
As mentioned in the <a href="#command-line">command line arguments</a> section,
command line options always take priority over `yuidoc.json` configuration values.</p>

The `yuidoc.json` file must reside in a directory somewhere under where you execute `yuidoc`. 
YUIDoc will scan the tree for this file before doing anything else.

A short example `yuidoc.json` file would resemble:

```
{
    "name": "The Foo API",
    "description": "The Foo API: a library for doing X, Y, and Z",
    "version": "1.2.1",
    "url": "http://example.com/",
    "options": {
        "outdir": "../build/apidocs"
    }
}
```

See below for <a href="#example-yui-3-library-api">more</a> <a href="#example-yuidoc-api">examples</a>.

### yuidoc.json Fields

#### General YUIDoc Project Information

<table>
<tr>
    <th>Name</th>
    <th>Description</th>
</tr>
<tr>
    <td><code>name</code></th>
    <td>A short name for the project.</td>
</tr>
<tr>
    <td><code>description</code></th>
    <td>A one or two sentence description of the project.</td>
</tr>
<tr>
    <td><code>version</code></th>
    <td>The project's current version, as some kind of meaningful string.</td>
</tr>
<tr>
    <td><code>url</code></th>
    <td>The project's primary URL. This does not necessarily have to be the URL of the generated API documentation.</td>
</tr>
<tr>
    <td><code>logo</code></th>
    <td>
        The logo to add to the header of all generated HTML documentation. 
        If you do not provide a header, YUIDoc will use the YUI logo by default.
    </td>
</tr>
</table>

#### YUIDoc Options

Within the `options` object, you can provide any of the following fields:

<table>
<tr>
    <th>Name</th>
    <th>Description</th>
</tr>
<tr>
    <td><code>linkNatives</code></th>
    <td>Selects whether to autolink native types such as <code>String</code> and <code>Object</code> over to the Mozilla Developer Network. </td>
</tr>
<tr>
    <td><code>attributesEmit</code></th>
    <td>
        Selects whether YUIDoc should autogenerate documentation for change events 
        generated by the <a href="http://yuilibrary.com/yui/docs/api/classes/Attribute.html">YUI Attribute API</a>.
        When a YUI attribute <code>foo</code> changes its value, 
        YUI automatically fires a custom event named <code>fooChange</code>.
        Setting <code>attributesEmit</code> to <code>true</code> instructs YUIDoc to
        automatically generate documentation for each of these events.
        You can set this value to <code>false</code> if you think that your audience
        is well aware of change events and would not benefit from this extra verbiage.
    </td>
</tr>
<tr>
    <td><code>selleck</code></th>
    <td>
        Selects whether to add <a href="http://rgrove.github.com/selleck/">Selleck</a> metadata. 
        If <code>true</code>, YUIDoc searches for a <code>component.json</code> file above the source tree and 
        attaches that data to the module data as extra information.
    </td>
</tr>
<tr>
    <td><code>ignorePaths</code></th>
    <td>Specifies an array of string paths to ignore when using shell globbing. This only removes top level items from the list to initally scan. Use <code>exclude</code> to remove specific directories.</td>
</tr>
<tr>
    <td><code>exclude</code></th>
    <td>Specify a comma separated list of names you want to exclude from parses when YUIDoc recurses the source tree.</td>
</tr>
<tr>
    <td><code>paths</code></th>
    <td>
        Specifies a single string <code>glob</code> or array of globs 
        to use when searching the source tree for docs to parse.
    </td>
</tr>
<tr>
    <td><code>outdir</code></th>
    <td>Specifies the directory in which to place the rendered HTML files and assets.</td>
</tr>
<tr>
    <td><code>tabtospace</code></th>
    <td>Specifies the number of spaces each tab character in source code should be converted to when using YUIDoc's source code view. The default is 8.</td>
</tr>
<tr>
    <td><code>external.data</code></th>
    <td>
        Provides a link to an external <code>data.json</code> file to merge into the local api docs. 
        For more information, refer to the <a href="#adding-external-yuidoc-data-to-your-project">external data example</a>.
    </td>
</tr>
<tr>
    <td><code>markdown</code></td>
    <td>
        Options to pass to markdown-it, the Markdown compiler used to compile API descriptions.
        See the <a href="https://markdown-it.github.io/markdown-it/#MarkdownIt.new">markdown-it API</a> for details.
    </td>
</tr>
<tr>
    <td><code>preprocessor</code></td>
    <td>
        Specifies the array of your preprocessor script or npm package like <code>yuidoc-preprocessor-foo</code>
        that implements a preprocessor. See the <a href="#example-using-preprocessor">example used preprocessor</a>.
    </td>
</tr>
</table>


### Example: YUI 3 Library API

This sample `yuidoc.json` file is used in the <a href="http://yuilibrary.com/">YUI 3</a>
project:

```
{
    "name": "YUI 3",
    "description": "YUI 3 JavaScript Framework",
    "version": "3.5.0",
    "url": "http://yuilibrary.com/",
    "options": {
        "linkNatives": "true",        
        "attributesEmit": "true",
        "selleck": "true",
        "ignorePaths": [ "simpleyui" ],
        "paths": "*/js",
        "outdir": "../api-js"
    }
}
```

### Example: YUIDoc API

This sample `yuidoc.json` file is used in the YUIDoc project itself:

```
{
  "name": "YUIDoc",
  "description": "YUIDoc documentation tool written in JavaScript",
  "version": "0.2.38",
  "url": "http://yuilibrary.com/projects/yuidoc",
  "logo": "http://yuilibrary.com/img/yui-logo.png",
  "options": {
    "external": {
      "data": "http://yuilibrary.com/yui/docs/api/data.json"
    },
    "linkNatives": "true",
    "attributesEmit": "true",
    "paths": [
      "./lib"
    ],
    "outdir": "./output/api"
  }
}
```

### Example: Using preprocessor

This sample `yuidoc.json` file used preprocessor option:

```
{
  "name": "My Project",
  "version": "1.0.0",
  "options": {
    "paths": "src",
    "preprocessor": ["./path/to/custom_doc_preprocessor.js", "yuidoc-preprocessor-foo"]
  }
}
```
