# Themes

YUIDoc uses <a href="http://handlebarsjs.com/">Handlebars.js</a> to render its templates. 
For easy customization, YUIDoc's default templates provide a specific set of file overrides.

## Directories

The default theme consists of `assets/`, `layouts/` and `partials/` directories, 
along with a `theme.json` file that describes theme-related variables, 
such as the JS and CSS to load.

```terminal

themes/
    default/
        assets/             //Project assets, css, js
        layouts/
            *.handlebars    //Handlebars files for layouts
        partials/
            *.handlebars    //Handlebars files for partials
        theme.json   //JSON file with theme variables
```

### Layouts

A YUIDoc template has two primary layout files: `main.handlebars` and `xhr.handlebars`.

<table>
<tr>
    <th><code>main</code></th>
    <td>
        Provides a complete wrapper around every rendered page. 
        <code>main.handlebars</code> includes the full HTML header and footer markup, CSS, and JS
        for every YUIDoc API page.
    </td>
</tr>
<tr>
    <th><code>xhr</code></th>
    <td>
        Provides a smaller layout for the built-in doc server to use when requesting an individual page via XHR. 
        This enables the browser to refresh just the content pane and avoid loading the complete  markup for the entire page.
        The <code>xhr</code> template enables YUIDoc to progressively enhance the API documentation in an efficient manner.
    </td>
</tr>
</table>

<h3>Partials</h3>

<p>
    For each section of the layout that derives from parsed YUIDoc comment data, 
    YUIDoc provides a Handlebars partial. 
</p>

<table>
<tr>
    <th><code>index</code></th>
    <td>Renders the main index content.</td>
</tr>
<tr>
    <th><code>sidebar</code></th>
    <td>Renders the tabview containing the lists of classes and modules.</td>
</tr>
<tr>
    <th><code>options</code></th>
    <td>Renders the filter options at the top of the page, which enable the user to hide and show private methods, inherited methods, and so on.</td>
</tr>
<tr>
    <th><code>attrs</code></th>
    <td>Renders documentation for an individual YUI Attribute.</td>
</tr>
<tr>
    <th><code>classes</code></th>
    <td>Renders documentation for an individual class.</td>
</tr>
<tr>
    <th><code>events</code></th>
    <td>Renders documentation for an individual event.</td>
</tr>
<tr>
    <th><code>files</code></th>
    <td>Renders the API's source files.</td>
</tr>
<tr>
    <th><code>method</code></th>
    <td>Renders documentation for an individual method.</td>
</tr>
<tr>
    <th><code>module</code></th>
    <td>Renders documentation for an individual module.</td>
</tr>
<tr>
    <th><code>props</code></th>
    <td>Renders documentation for an individual property.</td>
</tr>
</table>


## Overriding a Partial/Layout

YUIDoc's `--themedir` option specifies a directory containing 
layouts and partials that override the default theme. For example:

```terminal
$ yuidoc --themedir ./mytheme
```

causes YUIDoc to inspect the directory `./mytheme` for template overrides.
If this directory contains an override such as `./mytheme/partials/method.handlebars`,
YUI will parse its internal templates first, then apply the custom `method.handlebars` partial.
If a theme has no explicit override for a given template file, 
YUIDoc simply falls back to using the default layout or partial.
