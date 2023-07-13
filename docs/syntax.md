# Syntax

YUIDoc's syntax should be familiar if you've used Javadoc, JSDoc, Doxygen, or other documentation generator tools. YUIDoc relies on <dfn>tags</dfn> such as `@param` or `@return` embedded in comment blocks that start with `/**` and end with `*/`. See <a href="#comment-styles">comment styles</a> for more information. It includes a small number of tags for documenting specific YUI features, but most tags are generic enough to use with any object-oriented language.</p>

<b>IMPORTANT:</b> YUIDoc only parses YUIDoc comment blocks, not source code. This keeps YUIDoc relatively simple and language agnostic. However, it also means you must declare everything to YUIDoc explicitly. A code snippet will not display as a "method" or "class" until you describe it as such. A corollary is that YUIDoc will never generate empty, "stub" doc entries. for API members that lack comment blocks.

## Basic Requirements

A given comment block must contain one (and only one) <a href="#primary-tags">primary tag</a> such as `@class` or `@method`, and zero or more <a href="#secondary-tags">secondary tags</a> such as `@param`, `@type`, and `@extends`. Some secondary tags can be used in any comment block, while others only make sense alongside a particular primary tag.

A source tree must contain at least one comment block with a `@module` tag.

Each module must have at least one comment block with a `@class` tag.

Each class may then have zero or more comment blocks with an `@attribute`, `@class`, `@event`, `@method`, or `@property` tag.

## Primary Tags

Each comment block must have one (and only one) of the following tags:

### `module`
```
/**
 * Provides the base Widget class...
 *
 * @module widget
 */
```
Indicates that the block describes a group of related classes.
For example, YUI's <code>app</code> module includes classes such as <code>App.Base</code>, <code>Model</code>, and <code>Router</code>.
You can optionally break modules up into submodules.

YUIDoc requires you to provide at least one module per source tree.
Since there isn't always an obvious place to insert module documentation in JavaScript source,
the convention is to declare your module at the top of the file that contains your module's "primary" or "base" class.

See also:
<a href="#class"><code>@class</code></a>,
<a href="#for"><code>@for</code></a>,
<a href="#main"><code>@main</code></a>,
<a href="#submodule"><code>@submodule</code></a>.

### `main`

```
/**
 * Provides more features for the widget module...
 *
 * @module widget
 * @submodule widget-foo
 * @main widget
 */
```
When YUIDoc parses a module's directory, there may be several files in this directory that
provides documentation for that module and it's submodules. YUIDoc will attempt to determine
which module contains the main description for this module. If it has trouble doing that,
you can add a <code>@main</code> tag to your module/submodule description and YUIDoc will use this block as
the main module description on the modules API landing page.

### `class`
```
/**
 * A utility that brokers HTTP requests...
 *
 * @class IO
 * @constructor
 */
function IO (config) {
```
Indicates that the block describes a class.
In JavaScript, this is generally an object with a constructor function.
The value of <code>@class</code> should be the string that identifies the functional class on its parent object.
For example, the <code>@class</code> for <code>Y.DD.Drag</code> would be <code>Drag</code>
(and its <a href="#namespace"><code>@namespace</code></a> would be <code>DD</code>).

YUIDoc expects methods, properties, attributes, and events to belong to a class,
so in general you must provide at least one class for each module in your source tree.
A <code>@class</code> block should reside just above the class's constructor function,
and above all methods, events, properties, and attributes that belong to the class.

A <a href="#class"><code>@class</code></a> tag should be paired with
either a <code>@constructor</code> tag or a <code>@static</code> tag.

See also:
<a href="#constructor"><code>@constructor</code></a>,
<a href="#extends"><code>@extends</code></a>,
<a href="#extensionfor"><code>@extensionfor</code></a>,
<a href="#for"><code>@for</code></a>,
<a href="#module"><code>@module</code></a>,
<a href="#namespace"><code>@namespace</code></a>,
<a href="#static"><code>@static</code></a>,
<a href="#uses"><code>@uses</code></a>.


### element
```
/**
 * This is the foo element description...
 *
 * @element x-foo
 */
```

Indicates that the block describes a Custom Element.
The <a href="#attribute"><code>@attribute</code></a> tag works as a attribute of the element
when you specify a <code>@element</code> tag. You can also specify the
<a href="#parents"><code>@parents</code></a>, <a href="#contents"><code>@contents</code></a>,
and <a href="#interface"><code>@interface</code></a> tag for the element.

See also:
<a href="#attribute"><code>@attribute</code></a>,
<a href="#parents"><code>@parents</code></a>,
<a href="#contents"><code>@contents</code></a>,
<a href="#interface"><code>@interface</code></a>.

### `method`

```
/**
 * Returns this model's attributes as...
 *
 * @method toJSON
 * @return {Object} Copy of ...
 */
toJSON: function () {
```

Indicates that the block describes a method for the current class.
By default, the "current" class is the last class that YUIDoc parsed, but
you can reset this with the <a href="#for"><code>@for</code></a> tag.

A <code>@method</code> block should always reside directly above the method's definition.
At a minimum, you should also document any
parameters (<a href="#param"><code>@param</code></a>) and
return values (<a href="#return"><code>@return</code></a>).

See also:
<a href="#chainable"><code>@chainable</code></a>,
<a href="#class"><code>@class</code></a>,
<a href="#constructor"><code>@constructor</code></a>,
<a href="#for"><code>@for</code></a>,
<a href="#param"><code>@param</code></a>,
<a href="#return"><code>@return</code></a>,
<a href="#throws"><code>@throws</code></a>,
<a href="#static"><code>@static</code></a>.

### `event`
```
/**
 * Fired when an error occurs...
 *
 * @event error
 * @param {String} msg A description of...
 */
var EVT_ERROR = 'error',
```
Indicates that the block describes a custom event that the class can fire
at some interesting moment of code execution.
An <code>@event</code> block is somewhat similar to a <a href="#param"><code>@method</code></a> block,
except that <a href="#return"><code>@return</code></a> is irrelevant, and
<a href="#param"><code>@param</code></a> is used to describe properties hanging off
the event object that callbacks listening for the event receive.

Ideally, an <code>@event</code> block should reside above the code that defines the event,
even if that code is just a simple string declaration.
If you find that your <code>@event</code> block is "floating in space,"
you should at least place it underneath the class that owns the event,
grouped with any other events that the class can fire.

See also:
<a href="#bubbles"><code>@bubbles</code></a>,
<a href="#class"><code>@class</code></a>,
<a href="#for"><code>@for</code></a>,
<a href="#param"><code>@param</code></a>.


### `property`
```
/**
 * Template for this view's container...
 *
 * @property containerTemplate
 * @type String
 * @default "<div/>"
 */
containerTemplate: '<div/>',
```
Indicates that the block describes a property belonging to the current class.

As with methods, a <code>@property</code> block should always reside
directly above the point where the property is defined.
At a minimum, you should also provide the property's <code>@type</code>,
even if the value is <code>"any"</code> or <code>"mixed"</code>.

See also:
<a href="#attribute"><code>@attribute</code></a>,
<a href="#default"><code>@default</code></a>,
<a href="#class"><code>@class</code></a>,
<a href="#for"><code>@for</code></a>,
<a href="#type"><code>@type</code></a>.


### `attribute`
```
/**
 * Indicates whether this Widget
 * has been rendered...
 *
 * @attribute rendered
 * @readOnly
 * @default false
 * @type boolean
 */
ATTRS[RENDERED] = {
```
[YUI-specific] Indicates that the block describes a managed configuration attribute.
An attribute is an object created and managed by the YUI
<a href="http://yuilibrary.com/yui/docs/api/classes/Attribute.html"><code>Attribute</code> API</a>.
It is a kind of "super-property", with getters, setters, and other nifty features,
including the ability to automatically fire change events.

An <code>@attribute</code> block should reside directly above the definition of the attribute,
whether that is inside a <code>Y.Base</code> object's <code>ATTRS</code> property or elsewhere.
Note that if your <code>yuidoc.json</code> file sets <code>attributesEmit</code> to <code>true</code>,
YUI will automatically generate documentation for the attribute's change events throughout the source tree,
with no extra YUIDoc comments needed from you.

If you specify a <a href="#element"><code>@element</code></a> tag, the <code>@attribute</code> tag works as
a attribute of the element.

See also: <a href="#element"><code>@element</code></a>,
<a href="#property"><code>@property</code></a>,
<a href="#default"><code>@default</code></a>,
<a href="#class"><code>@class</code></a>,
<a href="#for"><code>@for</code></a>,
<a href="#type"><code>@type</code></a>,
<a href="#required"><code>@required</code></a>,
<a href="#optional"><code>@optional</code></a>.

## Secondary tags

After choosing one of the five primary tags, you can further document a module, class, method, event or property with one or more of the following secondary tags.

<table>
<tr>
    <th>Name</th>
    <th>Example</th>
    <th>Description</th>
</tr>
<tr id="submodule">
    <td><code>submodule</code></td>
    <td>
```
/**
 * @module app
 * @submodule view
 */
```
    </td>
    <td>
        <p>Specifies that the module is actually a submodule of some parent module.
        For example, the <code>app-transitions</code> module is a submodule of the larger <code>app</code> module.</p>

        <p>In YUI, submodules enable you to make very fine-grained choices about loading code.
        For example, the <code>foo</code> module might have a minimal <code>foo-core</code> or <code>foo-base</code> submodule
        that supplies <code>foo</code>'s basic functionality,
        plus additional <code>foo-*</code> modules that carry optional features.
        Using the YUI Loader, you can choose to load just <code>foo-core</code>,
        <code>foo-core</code> plus a couple of extra modules,
        or the entire <code>foo</code> "rollup".</p>

        <p>
            See also:
            <a href="#module"><code>@module</code></a>.
        </p>
    </td>
</tr>
<tr id="namespace">
    <td><code>namespace</code></td>
    <td>
```
/**
 * @namespace Test.Mock
 */
```
    </td>
    <td>
        <p>Specifies a class's namespace.
        The <code>@namespace</code> should <em>not</em> include the "root" or "global" object
        that your entire library hangs off of.
        For example, <code>Y.DD.Drag</code> has
        a <a href="#class"><code>@class</code></a> of <code>Drag</code>
        and a <code>@namespace</code> of <code>DD</code>, not <code>Y.DD</code>.</p>

        <p>Supplying a <code>@namespace</code> enables you to refer to the class in YUIDoc using just the simple class name.</p>

        <p>
            See also:
            <a href="#class"><code>@class</code></a>.
        </p>
    </td>
</tr>
<tr id="extends">
    <td><code>extends</code></td>
    <td>
```
/**
 * @class View
 * @constructor
 * @extends Base
 */
```
   </td>
   <td>
        <p>Specifies that the class inherits members from a parent class,
        perhaps using <a href="http://yuilibrary.com/yui/docs/api/classes/YUI.html#method_extend"><code>Y.extend()</code></a>,
        <a href="http://yuilibrary.com/yui/docs/api/classes/Base.html#method_create"><code>Y.Base.create()</code></a>,
        or similar methods.
        YUIDoc will generate API documentation for
        methods, properties, events, and attributes inherited from the parent class,
        and link back to the parent class's documentation.
        In the default YUIDoc theme, users can toggle whether inherited members should display.</p>

        <p>
            See also:
            <a href="#class"><code>@class</code></a>,
            <a href="#extensionfor"><code>@extensionfor</code></a>,
            <a href="#uses"><code>@uses</code></a>.
        </p>
   </td>
</tr>
<tr id="config">
    <td><code>config</code></td>
    <td>
```
/**
 * @config docScrollX
 * @type Number
 */
```
    </td>
    <td>
        <p>[YUI-specific] Alias for <a href="#attribute"><code>@attribute</code></a>.
        In older versions of YUI, <code>@config</code> was a slightly different take on attributes,
        but the two concepts have merged.
        Modern YUIDoc comments should use <code>@attribute</code> instead.</p>
    </td>
</tr>
<tr id="constructor">
    <td><code>constructor</code></td>
    <td>
```
/**
 * @class IO
 * @constructor
 */
```
    </td>
    <td>
        <p>Indicates that the class is instantiable
        (created with the <code>new</code> keyword).
        A <a href="#class"><code>@class</code></a> tag should be paired with
        either a <code>@constructor</code> tag or a <code>@static</code> tag.</p>
        <p>
            See also:
            <a href="#class"><code>@class</code></a>,
            <a href="#static"><code>@static</code></a>.
        </p>
    </td>
</tr>
<tr id="static">
    <td><code>static</code></td>
    <td>
```
/**
 * YUI user agent detection...
 *
 * @class UA
 * @static
 */


```
    </td>
    <td>
        <p>Indicates that the method or class is static:</p>
        <ul>
            <li>For methods, indicates that the method is meant to be
            called without instantiating the class:
            <code>var node = Y.Node.create('<div/>');</code></li>
            <li>For classes, indicates that you should not
            instantiate the class with <code>new</code>.
            You can call all of the class's methods statically.
        </ul>
        <p>A <a href="#class"><code>@class</code></a> tag should be paired with
        either a <code>@constructor</code> tag or a <code>@static</code> tag.</p>
        <p>
            See also:
            <a href="#class"><code>@class</code></a>,
            <a href="#constructor"><code>@constructor</code></a>,
            <a href="#method"><code>@method</code></a>.
        </p>
    </td>
</tr>
<tr id="final">
    <td><code>final</code></td>
    <td>
```
/**
 * Identifies state changes
 * originating from...
 *
 * @property SRC_REPLACE
 * @type String
 * @static
 * @final
 */
```
    </td>
    <td>
        <p>Indicates that the property or attribute is a constant and should not be changed.</p>
        <p>
            See also:
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#property"><code>@property</code></a>,
            <a href="#readOnly"><code>@readOnly</code></a>,
            <a href="#writeOnce"><code>@writeOnce</code></a>.
        </p>
    </td>
</tr>
<tr id="readOnly">
    <td><code>readOnly</code></td>
    <td>
```
/**
 * The current default button
 * as configured through...
 *
 * @attribute defaultButton
 * @type Node
 * @default null
 * @readOnly
 */
```
    </td>
    <td>
        <p>[YUI-specific] Indicates that the attribute is configured with the
        <a href="http://yuilibrary.com/yui/docs/api/classes/Attribute.html#method_addAttr"><code>readOnly</code></a> property
        and cannot be changed by calling the
        <a href="http://yuilibrary.com/yui/docs/api/classes/Attribute.html#method_set"><code>set()</code></a> method.
        Read-only attributes should always document their <a href="#default"><code>@default</code></a> value.</p>

        <p>Sometimes used with properties, as an alias for <a href="#final"><code>@final</code></a>.</p>

        <p>
            See also:
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#default"><code>@default</code></a>,
            <a href="#final"><code>@final</code></a>,
            <a href="#property"><code>@property</code></a>,
            <a href="#required"><code>@required</code></a>,
            <a href="#optional"><code>@optional</code></a>,
            <a href="#writeOnce"><code>@writeOnce</code></a>.
        </p>
    </td>
</tr>
<tr id="writeOnce">
    <td><code>writeOnce</code></td>
    <td>
```
/**
 * Diameter of the circular
 * background object. Other
 * objects scale accordingly.
 * Set this only before
 * rendering.
 *
 * @attribute diameter
 * @type {Number} number of px
 * in diameter
 * @default 100
 * @writeOnce
 */
```
    </td>
    <td>
        <p>[YUI-specific] Indicates that the attribute is configured with the
        <a href="http://yuilibrary.com/yui/docs/api/classes/Attribute.html#method_addAttr"><code>writeOnce</code></a> property
        and can only be set once --
        by applying a <a href="#default"><code>@default</code></a>,
        by setting the value in the constructior,
        or by calling the
        <a href="http://yuilibrary.com/yui/docs/api/classes/Attribute.html#method_set"><code>set()</code></a> method
        for the first time.</p>

        <p>
            See also:
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#default"><code>@default</code></a>,
            <a href="#final"><code>@final</code></a>,
            <a href="#required"><code>@required</code></a>,
            <a href="#optional"><code>@optional</code></a>,
            <a href="#readOnly"><code>@readOnly</code></a>.
        </p>
    </td>
</tr>
<tr id="optional">
    <td><code>optional</code></td>
    <td>
```
/**
 * An optional attribute,
 * not required for proper
 * use.
 *
 * @attribute extras
 * @type {Object} extra data
 * @optional
 */
```
    </td>
    <td>
        <p>
        [YUI-specific] Indicates that the attribute is not
        required to be provided for proper use of this class.
        </p>

        <p>
            See also:
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#default"><code>@default</code></a>,
            <a href="#final"><code>@final</code></a>,
            <a href="#required"><code>@required</code></a>,
            <a href="#readOnly"><code>@readOnly</code></a>.
        </p>
    </td>
</tr>

<tr id="required">
    <td><code>required</code></td>
    <td>
```
/**
 * A required attribute
 * that is required for proper
 * use, module will likely fail
 * if this is not provided.
 *
 * @attribute url
 * @type {String} url to fetch remote data from
 * @required
 */
```
    </td>
    <td>
        <p>
        [YUI-specific] Indicates that the attribute is
        required to be provided for proper use of this class.
        </p>

        <p>
            See also:
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#default"><code>@default</code></a>,
            <a href="#final"><code>@final</code></a>,
            <a href="#optional"><code>@optional</code></a>,
            <a href="#readOnly"><code>@readOnly</code></a>.
        </p>
    </td>
</tr>


<tr id="param">
    <td><code>*param</code></td>
    <td>
```
/**
 * @param {String} name An
 * Attribute name or
 * object property path.
 */
```

```
/**
 * @param {Object} [options] Data
 * to be mixed into the event
 * facade of the <code>change</code>
 * event(s) for these attributes.
 * @param {Boolean} [options.silent]
 * If <code>true</code>, no <code>change</code> event
 * will be fired.
 */
```
    </td>
    <td>
        <p>Defines a parameter for an ordinary <a href="#method"><code>@method</code></a>,
        a parameter for a <a href="#constructor"><code>@constructor</code></a>
        (generally defined inside a <a href="#class"><code>@class</code></a> block),
        <em>or</em> a property that resides on an <a href="#method"><code>@event</code></a> object.
        Can take either of the forms:</p>

        <ul>
            <li><code>@param {type} name description</code></li>
            <li><code>@param name {type} description</code></li>
        </ul>

        <p>The <code>{type}</code> is optional, but if you include it,
        you must surround it in curly braces so that YUIDoc
        can distinguish it from the <code>name</code>.
        The <code>name</code> also has optional syntax:</p>
        <ul>
            <li><code>[name]</code> &mdash; optional parameter</li>
            <li><code>[name=foo]</code> &mdash; default value is foo</li>
            <li><code>...name</code> &mdash; placeholder for 1..n args</li>
            <li><code>[...name]</code> &mdash; placeholder for 0..n args</li>
        </ul>

        <p>As shown in the example, you can also nest <code>@param</code> tags.
        This enables you to document object parameters that
        have their own particular nested structure.</p>

        <p>
            See also:
            <a href="#class"><code>@class</code></a>,
            <a href="#constructor"><code>@constructor</code></a>,
            <a href="#event"><code>@event</code></a>,
            <a href="#method"><code>@method</code></a>,
            <a href="#return"><code>@return</code></a>.
        </p>
    </td>
</tr>
<tr id="return">
    <td><code>return</code></td>
    <td>
```
/**
 * @method generateClientId
 * @return {String} Unique clientId.
 */
```
    </td>
    <td>
        <p>Specifies a method's return value.
        A <code>@return</code> tag has the structure <code>@return {type} description</code>.
        The <code>{type}</code> is optional.</p>

        <!--p>If a return value is an object with a complex structure,
        you can <a href="#param">nest <code>@param</code> tags</a>
        underneath the <code>@return</code> value.</p-->

        <p>
            See also:
            <a href="#method"><code>@method</code></a>,
            <a href="#param"><code>@param</code></a>.
        </p>
    </td>
</tr>
<tr id="throws">
    <td><code>throws</code></td>
    <td>
```
/**
 * @method generateClientId
 * @throws {Error} An error.
 */
```
    </td>
    <td>
        <p>Specifies an error which method throws.
        A <code>@throws</code> tag has the structure <code>@throws {type} description</code>.
        The <code>{type}</code> is optional.</p>
        <p>
            See also:
            <a href="#method"><code>@method</code></a>,
            <a href="#return"><code>@return</code></a>.
        </p>
    </td>
</tr>
<tr id="for">
    <td><code>for</code></td>
    <td>
```
/**
 * Some inner class 'foo'...
 *
 * @class foo
 * @for OuterClass
 */
```
```
/**
 * Some method 'bar'
 * disconnected from
 * its class 'FarawayClass'...
 *
 * @method bar
 * @for FarawayClass
 */
```
    </td>
    <td>
        <p>Sets YUIDoc's class scope.</p>

        <p>Using <code>@for OuterClass</code> in a <code>@class</code> block creates an inner class.
        YUIDoc will document methods and other items that follow that block
        as belonging to the inner class, but the inner class is correctly
        shown as belonging to its parent outer class.</p>

        <p>To close an inner class, add <code>@for OuterClass</code> (again!)
        to the <em>last</em>
        <code>@attribute</code>, <code>@event</code>, <code>@method</code>, or <code>@property</code> block
        in the inner class.
        This resets the YUIDoc parser to use <code>OuterClass</code>
        as the owner of subsequent items.</p>

        <p>If you are not inside an inner class,
        using <code>@for FarawayClass</code>
        in an <code>@attribute</code>, <code>@event</code>, <code>@method</code>, or <code>@property</code> block
        will attach all that item and subsequent items
        to the specified faraway class.
        This is useful when you have a module that attaches extra
        methods to a class's prototype,
        but the main class definition is in some entirely different file.</p>

        <p>
            See also:
            <a href="#class"><code>@class</code></a>,
            <a href="#method"><code>@method</code></a>.
        </p>
</td>
</tr>
<tr id="type">
    <td><code>type</code></td>
    <td>
```
/**
 * @type String
 */
```
```
/**
 * @type HTMLElement|Node|String
 */
```
    </td>
    <td>
        <p>Specifies the type of a property or attribute.
        You can specify a single type,
        a list of legal types separated by vertical bars,
        or if you are lazy, "any" or "mixed".</p>

        <p>
            See also:
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#default"><code>@default</code></a>,
            <a href="#property"><code>@property</code></a>.
        <p>
    </td>
</tr>
<tr id="private">
    <td><code>private</code></td>
    <td>
```
/**
 * Reference to the internal JSONP
 * instance used to make the queries.
 *
 * @private
 * @property _jsonp
 */
```
    </td>
    <td>
        <p>Indicates a member that should not be used externally.
        Although YUIDoc does not generate documentation for <code>@private</code> blocks,
        YUIDoc comments are still a nice, structured way to document internals in source code.
        All methods and properties are assumed to be public
        unless marked as private or protected.</p>

        <p>
            See also:
            <a href="#protected"><code>@protected</code></a>.
        <p>
    </td>
</tr>
<tr id="protected">
    <td><code>protected</code></td>
    <td>
```
/**
 * Removes the <code>container</code> from
 * the DOM and ...
 *
 * @method _destroyContainer
 * @protected
 */
```
    </td>
    <td>
        <p>Indicates a member that should not be modified
        by implementers unless they are creating a subclass.
        All methods and properties are assumed to be public
        unless marked as private or protected.</p>

        <p>
            See also:
            <a href="#private"><code>@private</code></a>.
        <p>
    </td>
</tr>
<tr id="requires">
    <td><code>requires</code></td>
    <td>
```
/**
 * @module event-simulate
 * @requires event
 */
```
    </td>
    <td>
        <p>[Uncommon] Identifies one or more dependencies in the module declaration.
        Can be a single module name or a comma-separated list.</p>

        <p>
            See also:
            <a href="#extends"><code>@extends</code></a>,
            <a href="#extensionfor"><code>@extensionfor</code></a>,
            <a href="#module"><code>@module</code></a>,
            <a href="#submodule"><code>@submodule</code></a>.
        <p>
    </td>
</tr>
<tr id="default">
    <td><code>default</code></td>
    <td>
```
/**
 * @default false
 */
```
    </td>
    <td>
        <p>Specifies the default value of a property or attribute.
        Should be paired with a <a href="#type"><code>@type</code></a> tag.</p>

        <p>
            See also:
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#property"><code>@property</code></a>,
            <a href="#type"><code>@type</code></a>.
        <p>
    </td>
</tr>
<tr id="uses">
    <td><code>*uses</code></td>
    <td>
```
/**
 * @class Panel
 * @constructor
 * @extends Widget
 * @uses WidgetAutohide
 * @uses WidgetButtons
...
 */
```
    </td>
    <td>
        <p>Specifies that the class has some other class's
        properties, methods, and other members mixed into its prototype,
        perhaps using <a href="http://yuilibrary.com/yui/docs/api/classes/YUI.html#method_mix"><code>Y.mix()</code></a>,
        <a href="http://yuilibrary.com/yui/docs/api/classes/Base.html#method_mix"><code>Y.Base.mix()</code></a>,
        <a href="http://yuilibrary.com/yui/docs/api/classes/Base.html#method_create"><code>Y.Base.create()</code></a>,
        or similar methods.
        YUIDoc will generate API documentation for
        methods, properties, events, and attributes mixed into the parent class,
        and link back to the parent class's documentation.
        In the default YUIDoc theme, users can toggle whether mixed in members should display.</p>

        <p>Note that <code>@uses</code> does not indicate inheritance.
        To establish an "is a" relationship, use <a href="#extends"><code>@extends</code></a>.
        Unlike <code>@extends</code>, you can provide multiple <code>@uses</code> tags. </p>

        <p>
            See also:
            <a href="#class"><code>@class</code></a>,
            <a href="#extends"><code>@extends</code></a>,
            <a href="#extensionfor"><code>@extensionfor</code></a>.
        </p>
    </td>
</tr>
<tr id="example">
    <td><code>*example</code></td>
    <td>
```
/**
 * @example
 *     model.set('foo', 'bar');
 */
```
    </td>
    <td>
        <p>Indicates a block of example code
        to be automatically parsed and displayed with
        YUIDoc's Markdown and code highlighting parser.
        Your code sample should be indented beneath the <code>@example</code> tag.
        YUIDoc displays all examples highlighted with
        <code><span></code> elements and other markup.</p>

        <p>A block may include multiple <code>@example</code> tags.</p>
    </td>
</tr>
<tr id="chainable">
    <td><code>chainable</code></td>
    <td>
```
/**
 * Renders this view ...
 *
 * @method render
 * @chainable
 */
render: function () {
    return this;
},
```
    </td>
    <td>
        <p>Indicates that a method returns <code>this</code> (the parent object),
        enabling you to chain it with other calls on the same object.</p>

        <p>
            See also:
            <a href="#method"><code>@method</code></a>.
        </p>
</tr>
<tr id="deprecated">
    <td><code>deprecated</code></td>
    <td>
```
/**
 * @property locale
 * @type String
 * @deprecated Use <code>config.lang</code>
 * instead.
 */
```
    </td>
    <td>
        <p>Indicates that the module, class, or member is deprecated
        and will be removed in a future release.
        You can optionally supply a string message
        describing what to use instead.</p>

        <p>
            See also:
            <a href="#beta"><code>@beta</code></a>,
            <a href="#since"><code>@since</code></a>.
        </p>
    </td>
</tr>
<tr id="since">
    <td><code>since</code></td>
    <td>
```
/**
 * @since 3.4.0
 */
```
    </td>
    <td>
        <p>Indicates that the module, class, or member
        was added to the source at the specified version.</p>

        <p>
            See also:
            <a href="#beta"><code>@beta</code></a>,
            <a href="#deprecated"><code>@deprecated</code></a>.
        </p>
    </td>
</tr>
<tr id="async">
    <td><code>async</code></td>
    <td>
```
/**
 * @async
 */
```
    </td>
    <td>
        <p>[Uncommon] Indicates that the method is
        asynchronous and requires a callback.</p>
    </td>
</tr>
<tr id="beta">
    <td><code>beta</code></td>
    <td>
```
/**
 * @beta
 */
```
    </td>
    <td>
        <p>Indicates that the method, class, or member is in beta
        and might undergo backwards-incompatible changes in the near future.</p>

        <p>
            See also:
            <a href="#deprecated"><code>@deprecated</code></a>,
            <a href="#since"><code>@since</code></a>.
        </p>
    </td>
</tr>
<tr id="bubbles">
    <td><code>bubbles</code></td>
    <td>
```
/**
 * Handles the mouseup DOM event...
 *
 * @event drag:mouseup
 * @bubbles DDM
 */
```
    </td>
    <td>
        <p>Specifies the default target that a custom event bubbles to.
        This is a useful tag if your API has a "manager" class that
        is responsible for capturing a set of related custom events.</p>

        <p>
            See also:
            <a href="#event"><code>@event</code></a>.
        </p>
    </td>
</tr>
<tr id="extensionfor">
    <td><code>extension</code><br><code>extensionfor</code><br><code>extension_for</code></td>
    <td>
```
/**
 * @class PjaxBase
 * @extensionfor Router
 */
```
    </td>
    <td>
        <p>Indicates that the class is an extension object
        designed to be optionally mixed into the specified class.</p>

        <p><code>@extensionfor</code> is <em>almost</em> the inverse of <a href="#uses"><code>@uses</code></a>.
        The key difference is that <code>@uses</code> means,
        "this class <em>always</em> has the 'used' class mixed into its prototype,"
        while <code>@extensionfor</code> means,
        "this class <em>can</em> be mixed into the 'extensionfor' class,
        but it isn't baked in by default."</p>

        <p>
            See also:
            <a href="#class"><code>@class</code></a>,
            <a href="#extends"><code>@extends</code></a>,
            <a href="#uses"><code>@uses</code></a>.
        </p>
    </td>
</tr>
<tr id="parents">
    <td><code>parents</code></td>
    <td>
```
/**
 * @element x-foo
 * @parents <body>
 */
```
    </td>
    <td>
        <p>It's a secondary tag for the <a href="#element"><code>@element</code></a> tag.
        Indicates that the parent element of the element you specified.</p>

        <p>
            See also:
            <a href="#element"><code>@element</code></a>,
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#contents"><code>@contents</code></a>,
            <a href="#interface"><code>@interface</code></a>.
        </p>
    </td>
</tr>
<tr id="contents">
    <td><code>contents</code></td>
    <td>
```
/**
 * @element x-foo
 * @contents <x-bar>
 */
```
    </td>
    <td>
        <p>It's a secondary tag for the <a href="#element"><code>@element</code></a> tag.
        Indicates that the element contains in the element you specified.</p>

        <p>
            See also:
            <a href="#element"><code>@element</code></a>,
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#parents"><code>@parents</code></a>,
            <a href="#interface"><code>@interface</code></a>.
        </p>
    </td>
</tr>
<tr id="interface">
    <td><code>interface</code></td>
    <td>
```
/**
 * @element x-foo
 * @interface XFooElement
 */
```
    </td>
    <td>
        <p>It's a secondary tag for the <a href="#element"><code>@element</code></a> tag.
        Indicates that the interface for the element you specified.</p>

        <p>
            See also:
            <a href="#element"><code>@element</code></a>,
            <a href="#attribute"><code>@attribute</code></a>,
            <a href="#parents"><code>@parents</code></a>,
            <a href="#contents"><code>@contents</code></a>.
        </p>
    </td>
</tr>
</table>

<p>A <strong>*</strong> indicates that you can supply multiple tags of that type in the same block.</p>

### Parsed but not in the theme yet
<p>
    The following tags are parsed by the <code>DocParser</code> but are not in the default theme yet.
</p>
<table>
<tr id="author">
    <td><code>author</code></td>
    <td>
```
```
    </td>
    <td>Author information about this item</td>
</tr>
<tr id="broadcast">
    <td><code>broadcast</code></td>
    <td>
```
```
    </td>
    <td>Event broadcasts to a large audience than scoped</td>
</tr>
<tr id="category">
    <td><code>*category</code></td>
    <td>
```
```
    </td>
    <td>Category to place this item into.</td>
</tr>
</table>

<h2 id="comment-styles">Comment Styles</h2>
<p>
  The comment blocks can start with any amount of whitespace, and
  optionally one or more asterisks. Valid examples include:
</p>
<p>
```
/**
 * Description
 * @method description
 */
```
</p>
<p>
```
/**
 * Description
 * @method description
**/
```
</p>
<p>
```
/**
Description
@method description
*/
```
</p>
<p>
```
/**
Description
@method description
**/
```
</p>

<h2>Extra formatting</h2>

<p>
    YUIDoc supports 3 main forms of formatting your documentation. HTML,
    <a href="http://daringfireball.net/projects/markdown/">Markdown</a> &amp; <a href="http://rgrove.github.com/selleck/">Selleck</a>.
</p>

<table>
<tr>
    <td><code>HTML</code></td>
    <td>Doc comments may contain standard HTML markup and YUIDoc will display it as is.</td>
</tr>
<tr>
    <td><code>Markdown</code></td>
    <td>Full <a href="http://daringfireball.net/projects/markdown/syntax">Markdown syntax</a>
    is also supported.
    </td>
</tr>
<tr>
    <td><code>Selleck</code></td>
    <td><a href="http://rgrove.github.com/selleck/">Selleck's</a> additional parsing is also supported.</td>
</tr>
</table>

<h3>Markdown and Code Highlighting</h3>

<p>
Inside any documentation block you may use Markdown or Selleck based markup. If you indent your code snippets,
YUIDoc will automatically wrap them in a code block and syntax highlight them for you.
</p>

```
/**
 * This is the __module__ description for the <code>YUIDoc</code> module.
 *
 *     var options = {
 *         paths: [ './lib' ],
 *         outdir: './out'
 *     };
 *
 *     var Y = require('yuidoc');
 *     var json = (new Y.YUIDoc(options)).run();
 *
 * @class YUIDoc
 * @main yuidoc
 */
```

<p>
This would render as:
</p>

<div class="intro">
<p>This is the <strong>module</strong> description for the <code>YUIDoc</code> module.</p>
```
    var options = {
        paths: [ './lib' ],
        outdir: './out'
    };

    var Y = require('yuidoc');
    var json = (new Y.YUIDoc(options)).run();
```
</div>

<h3>Cross-referencing Modules and Classes</h3>

<p>YUIDoc also includes a Handlebars <code>blockHelper</code> that enables you to
easily cross-reference classes and modules. It uses this pattern:
</p>

```
#crossLink "Class/item:type"

#crossLink "Foo/bar:event"
#crossLink "Foo/bar:attribute"
#crossLink "Foo/bar:method" --default
```

<p>
So, for example, if you include:
</p>

```
/**
 *
 * This module also uses \{{#crossLink "Foo"}}\{{/crossLink}}, where Foo is a class name.
 * Also see \{{#crossLink "myClass/Foo:method"}}\{{/crossLink}}, where myClass is a class name and Foo is a method on that class.
 *
 * This module uses \{{#crossLinkModule "widget"}}\{{/crossLinkModule}}, where widget is a module name
 *
 * This module also uses \{{#crossLink "Bar"}} an awesome class \{{/crossLink}} named Bar.
 */
```

<p>
This automatically generates an internal link to Foo's API reference page:
</p>

```
<p>
This module also uses <a href="../classes/Foo.html" class="crosslink">Foo</a>,
where Foo is a class or module name.
</p>
<p>
Also see <a href="../classes/myClass.html#method_Foo">Foo</a>, where myClass
is a class name and Foo is a method on that class.
</p>
<p>
This module uses <a href="../modules/widget.html">widget</a>, where widget is a module name
</p>
<p>
This module also uses <a href="../classes/Bar.html" class="crosslink">an awesome class</a>
named Bar.
</p>
```

<p>
You can also call <code>crossLinkRaw</code> to return only the HREF portion of the link, so you can link it
yourself.
</p>

<h3>Using custom Handlebars block helpers</h3>

<p>
You can tell <code>YUIDoc</code> to include custom <code>Y.Handlebars</code> helpers with the <code>-H</code> or <code>--helpers</code> command line arguments
(or <code>helpers</code> Array in the <code>yuidoc.json</code> file).

Here is an example <code>helper.js</code> file:

```
module.exports = {
    davglass: function(item) {
        return "Dav Glass says: " + item
    }
};
```

<p>
Now you can use the <code>davglass</code> helper inside your own docs like this:
</p>

```
/**
 * This is also a test \{{#davglass "Foo"}}\{{/davglass}}
 */
```

This will output this in your documentation:

```
<p>
 This is also a test Dav Glass says: Foo
</p>
```
