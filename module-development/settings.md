---
title: Module Settings
sort: 2
---

# Adding settings to your module

## Accessing the GooseMod settings interface

The GooseMod settings interface can be accessed in your modules by importing
`@goosemod/settings`. You can extend Discord's settings with more categories,
pages, and other elements by importing the right procedures.

## Adding a new settings page

When GooseMod loads all modules, it first creates a new category called
`GooseMod Modules`, so that all modules thereafter add their settings under
this category by default.

Adding your own settings page is pretty straightforward: once your module has
been completely loaded it will call its defined `onLoadingFinished` prodedure,
all that's needed to add a page is call `createItem` from the settings import.

Do remember to remove the settings pages you add when your module gets disabled
though.

First import `createItem`:

```js
import { createItem } from '@goosemod/settings';
```

Then in your module's `onLoadingFinished`, call to `createItem` like this:

```js
createItem('Page name', [
  'version',
  // field objects
]);
```

## Removing a settings page

When your module is disabled, it should undo all modifications made, to
restore the state of Discord prior to when it was enabled.

[Adding a settings page](#adding-a-new-settings-page) was easy enough, but
removing one is even easier.

First add `removeItem` to your imports from `@goosemod/settings`:

```js
import { createItem, removeItem } from '@goosemod/settings';
```

Then in your module's `onRemove`, call to `removeItem` like so:

```js
removeItem('Page name');
```

## Types of settings fields

Settings fields are objects, each having a `type` property defining its type,
a `text` property defining the field's text.

Most fields also have a `subtext` property to add additional information if
needed, for example: precising the default values of your settings. The way
you would define a fields subtext is like so:

```js
{
  // field definition elements
  subtext: "subtext goes there",
  // other field definition elements
}
```

### Divider field

A simple divider to separate your fields. Most other fields already add a
divider, so needing one is a more specific use case. They are defined like so:

```js
{
  type: 'divider';
}
```

```note
Divider fields **do not** use the `subtext` property. However they have a
`text` property.
```

```warning
The divider field's `text` property has not yet been experimented with
and documented.
```

### Header field

Header fields let you make categories in your settings page, they are defined
like so:

```js
{
  type: "header",
  text: "Header text"
}
```

```note
Header fields do not use the `subtext` property.
```

### Text field

Text fields let you write text without user controls, they are defined like so:

```js
{
  type: "text",
  text: "Write your text here"
}
```

### Button field

Button fields let you add simple buttons to launch actions, they are defined
like so:

```js
{
  type: "button",
  text: "Action",
  onclick: () => {
    // Do something.
  }
}
```

```note
Buttons do not use the `subtext` property.
```

```note
Button fields are pretty bare and only simple buttons. If you use them, you may
want to add a [text field](#text-field) before to explain their function and a
[divider field](#divider-field) afterwards to properly separate them from the
following fields.
```

```tip
Given that button fields are so bare, I'd recommend using
[text-button fields](#text-button-field) in most cases.
```

#### Text-Button field

A regular [text field](#text-field) with a smaller [button](#button-field).
They are defined like so:

```js
{
  type: "text-and-button",
  text: "Description",
  buttonText: "Action",
  onclick: () => {
    // Do something.
  }
}
```

```note
Unlike [button fields](#button-field), text-button fields include a
[divider field](#divider-field), so you don't need to add one after.
```

#### Text-Button-Danger field

Exactly like the [text-button field](#text-button-field), but the button has
the danger styling (red). They are defined like so:

```js
{
  type: "text-and-danger-button",
  text: "Description",
  buttonText: "Action",
  onclick: () => {
    // Do something.
  }
},
```

```note
Due to the design language surrounding them, you should only use these if the
action could have impactful consequences.
```

### Toggle field

Toggle fields let you make on/off switches for your basic settings, they are
defined like so:

```js
{
  type: "toggle",
  text: "Description",
  onToggle: value => {
    // Do something with value (true/false).
  },
  isToggled: () => {
    // Define what state the toggle switch should be in when loading the page.
    return false;
  }
}
```

#### Toggle-Text-Button field

A [toggle field](#toggle-field) with a small [button](#button-field), or a
[text-button field](#text-button-field) with a [toggle switch](#toggle-field).
They are defined like so:

```js
{
  type: "toggle-text-button",
  text: "Description",
  onToggle: value => {
    // Do something with value (true/false)
  },
  isToggled: () => {
    // Define what state the toggle switch should be in when loading the page.
    return false;
  },
  buttonText: "Action",
  onclick: () => {
    // Do something
  }
}
```

#### Toggle-Text-Button-Danger

Exactly like the [toggle-text-button field](#toggle-text-button-field), but
the button has the danger styling (red). They are defined like so:

```js
{
  type: "toggle-text-danger-button",
  text: "Description",
  onToggle: value => {
    // Do something with value (true/false)
  },
  isToggled: () => {
    // Define what state the toggle switch should be in when loading the page.
    return false;
  },
  buttonText: "Action",
  onclick: () => {
    // Do something
  }
}
```

```note
Due to the design language surrounding them, you should only use these if the
action could have impactful consequences.
```

### Colour field

Colour fields let you make colour pickers for your settings, they are defined
like so:

```js
{
  type: "text-and-color",
  text: "Description",
  oninput: value => {
    // Do something with value.
  },
  initialValue: () => {
    // Define what colour should be displayed when loading the page.
    return "#00000";
  }
}
```

```note
The colour picker doesn't support alpha channel (opacity) yet.
```

### Card field

```warning
Not yet experimented with or documented.
```

### Search field

```warning
Not yet experimented with or documented.
```

### Sidebar field

```warning
Not yet experimented with or documented.
```

### Custom field

Custom fields only support a single property: `element`. They are defined like
so:

```js
{
  type: "custom",
  element: (() => {
    // Return a DOM element
    let e = document.createElement("span");
    e.innerText = "Custom Element";
    return e;
  })()
}
```

```note
Custom fields can be used to make any kind of field that is not built into
GooseMod. For example, [here is a procedure building a text input field](https://github.com/NovaGM/Modules/blob/master/settings-experiment/custom-settings.js#L1-L57).
```

