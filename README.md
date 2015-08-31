v0.1.0
#`<zero-list-hero>`

It's hero time! A Polymer 1.0+ web component that hero-animates a sequence of
elements. I do this so you don't have to.

![demo.gif](https://zerodevx.github.io/zero-list-hero/images/demo.gif)

Uses `<neon-animation>` and `web-animations-js`. Automatic transitions with
built-in debouncer; cascaded bespoke entry-exit effects à la Google Play
Newsstand app; configurable animations; and almost entirely declarative.


### Let's get this money

Basic usage:

```html
<script src="bower_components/webcomponentsjs/webcomponents-lite.js"></script>
<link rel="import" href="bower_components/zero-list-hero/zero-list-hero.html">
...

<zero-list-hero>
  <!-- Dynamically append/remove elements inside here -->
  ...
  <div hero-id="hero1">I'm Tom</div>
  <div hero-id="hero3">I'm Mary</div>
  <div hero-id="hero2">I'm John</div>
  ...
</zero-list-hero>
```

With Polymer's `dom-repeat`:

```html
<zero-list-hero>
  <template is="dom-repeat" items="[[list]]">
    <my-card hero-id$="[[item.foo]]" foo$="[[item.foo]]" bar$="[[item.bar]]"></my-card>
  </template>
</zero-list-hero>
```


### Implementation notes

1. Assign a unique `hero-id` for every display element that is wrapped inside
the `<zero-list-hero>...</zero-list-hero>` tags.

2. If you're using Polymer's data-binding (eg. inside a `dom-repeat` template),
set your target attributes annotatively via the ***`$=`*** syntax.

3. Transitions are automatic when changes to any `hero` element is detected.
Multiple mutations are consolidated and debounced with a default delay of 500ms
(optionally change delay timing by setting `debounce-delay` attribute).

3. Things should *just work* under Polymer's ***shady DOM*** behavior. However,
under native ***shadow DOM*** behavior, automatic transitions do **not** work.
For the latter, assert the `shadow` attribute and manually call the `animate()`
method when hero elements change.


### Shady vs Shadow

Essentially, changes to contents inside the `<zero-list-hero>` tag are observed
through the *MutationObservers* API - when change is detected, the `animate()`
method is automatically called to start the transition.

Unfortunately, *mutation observers* are currently unable to detect changes to
distributed nodes under native shadow behavior. Ongoing spec discussions can be
found [here](https://groups.google.com/forum/#!msg/polymer-dev/GMYzuuqlQ7k/2-gxqwJdNpcJ)
and [here](https://bugzilla.mozilla.org/show_bug.cgi?id=1026236).

Therefore, if you are using `<zero-list-hero>` natively, detect changes
*outside* the component and manually call the `animate()` method. As a
recommended pattern,

`parent-element.html`

```html
<link rel="import" href="bower_components/zero-list-hero/zero-list-hero.html">
<link rel="import" href="elements/my-card.html">
<dom-module id="parent-element">
  <template>
    ...
    <zero-list-hero id="myhero" shadow>
      <template is="dom-repeat" items="{{list}}">
        <my-card hero-id$="{{list.id}}" card-id="{{item.id}}" card-name="{{item.name}}"></my-card>
      </template>
    </zero-list-hero>
    ...
  </template>
  <script>
    Polymer({
      is: "parent-element",
      properties: {
        list: { type: Array, value: function () { return []; } }
      },
      observers: [
        "listChanged(list.*)"
      ],
      listChanged: function (changeRecord) {
        this.$.myhero.animate();
      }
      ...
    });
  </script>
</dom-module>

```

To future-proof your application, it is possible to disable *MutationObserver* 
usage entirely by asserting the `shadow` attribute and wiring up your change 
observers manually.


### Other issues

1. Polymer has an ongoing issue retrieving distributed content nodes through
`Polymer.domAPI(content).getDistributedNodes()`. The bug has been filed
[here](https://github.com/Polymer/polymer/issues/2283). As a workaround,
`<zero-list-hero>` will first try retrieving nodes via vanilla js (which works
under ***Shady*** behavior), and will silently fall back to Polymer's `domAPI`
should ***Shadow*** behavior be detected.


### Demo

The demo suite is published in https://zerodevx.github.io/zero-list-hero/demos.html.

Included demos:

**Simple demo:** https://zerodevx.github.io/zero-list-hero/simple-demo.html

**Demo using `dom-repeat`:** https://zerodevx.github.io/zero-list-hero/dom-repeat-demo.html

**Demo under native shadow-dom behavior:** https://zerodevx.github.io/zero-list-hero/shadow-demo.html

**Go crazy with Material Design - a demo Newsreader App:** https://zerodevx.github.io/zero-list-hero/newsreader-demo.html


### Published properties

| Property        | Type    | Description |
|-----------------|---------|-------------|
| debounceDelay   | Number  | Debounce delay between consolidating changes to `hero` elements and actually starting the transition; defaults to 500ms. |
| cascadeDelay    | Number  | Animation delay between multiple additions or removals of `hero` elements; defaults to 50ms. |
| addAnimation    | String  | [Animation](https://github.com/PolymerElements/neon-animation/tree/master/animations) to use when `hero` elements are **added** into view; if unspecified, a special `zero-entry-animation` custom animation is used. |
| removeAnimation | String  | [Animation](https://github.com/PolymerElements/neon-animation/tree/master/animations) to use when `hero` elements are **removed** from view; if unspecified, a special `zero-exit-animation` custom animation is used. |
| shadow          | Boolean | If `true`, disables *MutationObserver* usage. Manually call the `animate()` method to trigger the transitions. |
| inTransit       | Boolean | Read-only convenience property; switches to `true` whenever a hero-transition is ongoing. |


### Public methods

| Method    | Parameters | Description |
|-----------|------------|-------------|
| animate() | *none*     | Starts the transition process. Under ***shady*** dom behavior, this method is automatically called whenever changes are detected by the *MutationObserver* API. |


### Installation

Install through bower.

    bower install --save zerodevx/zero-list-hero#^0.1.0

Alternatively, download the project as a ZIP file and unpack into your
components directory. Note that `zero-list-hero` depends on `Polymer`, Polymer
Element's `neon-animation` as well as their related dependencies.

Source provided as-is - no assumptions are made on your production build
workflow.


### Version history

1. 2015-08-19: v0.1.0 - initial commit.


