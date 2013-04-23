![fm logo](artwork/logo.jpg)

A modular view layout manager for client and server.

---

## Usage

#### Define your modules

Every module must have a module name, and a template function.

```js
var Layout = FruitMachine.define({
  module: 'layout',
  template: <template-function>
});

var Orange = FruitMachine.define({
  module: 'orange',
  template: <template-function>
});

var Apple = FruitMachine.define({
  module: 'apple',
  template: <template-function>,

  // Bind interactions
  onSetup: function() {
    this.el.addEventListener('click', function() {
      alert('apple clicked!');
    });
  }
});
```

#### Arrange your modules into a layout...

```js
var layout = {
  module: 'layout-a',
  children: [
    {
      id: 'child_1',
      module: 'apple',
      data: {
        title: 'Apple Title'
      }
    },
    {
      id: 'child_2',
      module: 'orange'
    }
  ]
};

var view = new FruitMachine.View(layout);
```

#### ...or instantiate them directly

```js
var view = new Apple();
```

#### Render your view

```js
view.render();
```

#### Inject it into the DOM

```js
view.inject(document.body);
```

#### Then set up any interactions

```js
view.setup();
```

### Or all at once

```js
view
  .setup()
  .inject(document.body)
  .render();
```

## Why not use Backbone?

- No deps
- Can be retrofitted into a parts of a legacy system, it's not a religion
- Serverside has been considered from the start
- It doesn't care how you manage your data
- Modules are designed to be reused in multiple contexts, you create layouts, not fixed views
- A little more structured than Backbone

## API

### View();

View constructor



### View#add();

Adds a child view(s) to another View.

Options:

 - `at` The child index at which to insert
 - `inject` Injects the child's view element into the parent's

### View#remove();

Removes a child view from
its current View contexts
and also from the DOM unless
otherwise stated.

Options:

 - `fromDOM` Whether the element should be removed from the DOM (default `true`)

*Example:*

    // The following are equal
    // apple is removed from the
    // the view structure and DOM
    layout.remove(apple);
    apple.remove();

    // Apple is removed from the
    // view structure, but not the DOM
    layout.remove(apple, { el: false });
    apple.remove({ el: false });

### View#id();

Returns a decendent module
by id, or if called with no
arguments, returns this view's id.

*Example:*

    myView.id();
    //=> 'my_view_id'

    myView.id('my_other_views_id');
    //=> View

### View#module();

Returns the first descendent
View with the passed module type.
If called with no arguments the
View's own module type is returned.

*Example:*

    // Assuming 'myView' has 3 descendent
    // views with the module type 'apple'

    myView.modules('apple');
    //=> View

### View#modules();

Returns a list of descendent
Views that match the module
type given (Similar to
Element.querySelectorAll();).

*Example:*

    // Assuming 'myView' has 3 descendent
    // views with the module type 'apple'

    myView.modules('apple');
    //=> [ View, View, View ]

### View#each();

Calls the passed function
for each of the view's
children.

*Example:*

    myView.each(function(child) {
        // Do stuff with each child view...
    });

### View#toHTML();

Templates the view, including
any descendent views returning
an html string. All data in the
views model is made accessible
to the template.

Child views are printed into the
parent template by `id`. Alternatively
children can be iterated over a a list
and printed with `{{{child}}}}`.

*Example:*

    <div class="slot-1">{{{id_of_child_1}}}</div>
    <div class="slot-2">{{{id_of_child_2}}}</div>

    // or

    {{#children}}
        {{{child}}}
    {{/children}}

### View#render();

Renders the view and replaces
the `view.el` with a freshly
rendered node.

Fires a `render` event on the view.

### View#setup();

Sets up a view and all descendent
views.

Setup will be aborted if no `view.el`
is found. If a view is already setup,
teardown is run first to prevent a
view being setup twice.

Your custom `setup()` method is called

Options:

 - `shallow` Does not recurse when `true` (default `false`)

### View#teardown();

Tearsdown a view and all descendent
views that have been setup.

Your custom `teardown` method is
called and a `teardown` event is fired.

Options:

 - `shallow` Does not recurse when `true` (default `false`)

### View#destroy();

Completely destroys a view. This means
a view is torn down, removed from it's
current layout context and removed
from the DOM.

Your custom `destroy` method is
called and a `destroy` event is fired.

Options:

 - `fromDOM` Whether the view should be removed from DOM (default `true`)

### View#empty();

Destroys all children.



### View#inject();

Empties the destination element
and appends the view into it.



### View#appendTo();

Appends the view element into
the destination element.



### View#toJSON();

Returns a JSON represention of
a FruitMachine View. This can
be generated serverside and
passed into new FruitMachine(json)
to inflate serverside rendered
views.



### View#fire();

Proxies the standard Event.fire
method so that we can add bubble
functionality.

Options:

 - `propagate` States whether the event should bubble through parent views.



### Model();

Model constructor.



### Model#get();

Gets a value by key

If no key is given, the
whole model is returned.

### Model#set();

Sets data on the model.

Accepts either a key and
value, or an object literal.

### Model#clear();

CLears the data store.



### Model#destroy();

Deletes the data store.



### Model#toJSON();

Returns a shallow
clone of the data store.




## License
Copyright (c) 2012 Wilson Page
Licensed under the MIT license.