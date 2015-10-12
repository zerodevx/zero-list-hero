v1.0.0

# `<polymer-style-modules>`

A style-module implementation of common layout classes for Polymer 1.1+.

Because `/deep/`, `::shadow` and other piercing combinators are **deprecated**.
See [here](https://www.chromestatus.com/features/6750456638341120),
[here](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/68qSZM5QMRQ/pT2YCqZSomAJ)
and [here](https://www.w3.org/wiki/Webapps/WebComponentsApril2015Meeting).

### Let's get this money

Use inside your custom Polymer component:

`my-custom-element.html`

```html
<link rel="import" href="bower_components/polymer/polymer.html">
<link rel="import" href="bower_components/polymer-style-modules/iron-flex-layout-styles.html">

<dom-module id="my-custom-element">
  <template>
    <!-- Include using new Polymer 1.1+ style-module syntax -->
    <style include="iron-flex-layout-styles"></style>
    <style>
      /* My other styles applied to this element */
      ...
    </style>
    ...
  </template>
  <script>
    Polymer({
      is: "my-custom-element",
      ...
    });
  </script>
</dom-module>
```


### Included styles

| Style Module                 | Base Version                              | Description |
|------------------------------|-------------------------------------------|-------------|
| iron-flex-layout-styles.html | `PolymerElements/iron-flex-layout` v1.0.3 | Polymer's flexbox layout attributes. |
| paper-global-styles.html     | `PolymerElements/paper-styles` v1.0.11    | A set of base styles that are applied to the document and standard elements that match the Material Design spec. |
| paper-shadow-styles.html     | `PolymerElements/paper-styles` v1.0.11    | Shadow elevation definitions per Material Design spec. |
| paper-typography-styles.html | `PolymerElements/paper-styles` v1.0.11    | Typographic styles are provided matching the Material Design standard styles. |


### Installation

Install through bower.

    bower install --save zerodevx/polymer-style-modules#^1.0.0

Alternatively, download the project as a ZIP file and unpack into your
components directory. Note that `<polymer-style-modules>` uses
[`<font-roboto>`](https://github.com/PolymerElements/font-roboto).

Source provided as-is - no assumptions are made on your production build
workflow.


### Version history

1. 2015-09-23: v1.0.0 - initial commit.

