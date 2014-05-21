Revised code for my ALA article — Accessibility: The Missing Ingredient

================
== index.html ==
================

- Removed application role from `<body>`

  Non screen-readers still get the arrow navigation on load. In NVDA/Jaws, a user can now access the menu landmark and hit enter. After that, the up/down arrows move the user about the product list.

- Kept role="dialog" for semantic reasons and made all <td>s tabbable.

- changed:
  `role="list"` ==> `role="menu"`
and
  `role="listitem"` ==> `role="menuitem"`

  At first I tried Marco Zehe's suggestion (http://alistapart.com/comments/accessibility-the-missing-ingredient#336966) and attempted to use role="listbox" with role="option" for each section. This played well everywhere (almost). The best part was I could remove role="application" completely. However, in VoiceOver, items could not be added to the cart at all. This was obvoiusly a deal-breaker. The "add to cart" button seemed to not even exist within each listbox option. This sort of makes sense since, in HTML, you can't do anything to a select option other than select it. I believe this is why the children of role="option" did not count in VO are were not focusable/selectable/clickable. The best workaround I could find was to reintroduce the application role around the menu/menuitem structure.
```javascript
  <div role="application">
    <div id="product_sections" role="menu">
      {{#products}}
        <section tabindex="-1" role="menuitem">
        ...
        </section>
      {{/products}}
    </div>
  </div>
````
=================
== styles.scss ==
=================

- Removed visible focus styles for all elements except the active product section
- Decreased max-height of cart in mobile size
- Made cart close button and cart action buttons (+, -, x) more clickable
- Changed #my_cart overflow: scroll to overflow: auto
- Improved mobile styles for product list


============
== app.js ==
============

- Changed instances of `role="listitem"` to `role="menuitem"`
- Added tabindex='0' to the `<table>` and dynamically generated `<td>`s in the cart, so that the cart and its contents are read aloud
- Removed unwanted white space created in `updateItemCount()` text node

