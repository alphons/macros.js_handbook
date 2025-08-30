# Handbook for macros.js

This handbook provides a detailed guide on using `macros.js` in your web projects. `macros.js` is a lightweight JavaScript library that simplifies common DOM manipulations and event handling, similar to jQuery, but with a modern and compact approach. It is designed to work directly in the browser without dependencies.

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#installation)
3. [DOM Selectors](#dom-selectors)
4. [DOM Manipulation](#dom-manipulation)
5. [Event Handling](#event-handling)
6. [Currency Formatting](#currency-formatting)
7. [Example Project](#example-project)
8. [Comparison with jQuery](#comparison-with-jquery)
9. [Best Practices](#best-practices)

## Introduction
`macros.js` provides a set of helper functions and methods to select, manipulate, and manage events for DOM elements. It is a modern alternative to jQuery, focusing on simplicity and performance. It supports:
- Shorthand DOM selectors (`$`, `$$`, `$id`)
- Chainable DOM manipulation methods (`addClass`, `removeClass`, `toggleClass`, `show`, `hide`, `toggleVisibility`)
- Flexible event handling (`on`, `off`)
- Currency formatting (`formatCurrency`)

Unlike jQuery, `macros.js` is more compact and leverages modern JavaScript APIs like `querySelector` and `classList`.

## Installation
Include `macros.js` in your project by adding the script to your HTML file:

```html
<script src="macros.js"></script>
```

Place the `<script>` tag at the end of the `<body>` to ensure the DOM is fully loaded, or use the `onReady` function (see below).

## DOM Selectors
`macros.js` provides three global functions for selecting elements, similar to jQuery:

- **`$`**: Selects the first element matching a CSS selector.
  ```javascript
  const element = $('div.my-class'); // Similar to jQuery $('div.my-class')
  ```

- **`$$`**: Selects all elements matching a CSS selector as a `NodeList`.
  ```javascript
  const elements = $$('div.my-class'); // Similar to jQuery $('div.my-class')
  ```

- **`$id`**: Selects an element by its ID.
  ```javascript
  const element = $id('my-id'); // Similar to jQuery $('#my-id')
  ```

You can also use selectors within a specific element:
```javascript
const parent = $id('parent');
const child = parent.$('.child'); // Selects first .child within #parent
const children = parent.$$('.child'); // Selects all .child within #parent
```

## DOM Manipulation
`macros.js` offers chainable methods to manipulate DOM elements, similar to jQuery’s chainable API.

### Class Management
- **`addClass(className)`**: Adds a CSS class.
  ```javascript
  $id('my-element').addClass('highlight');
  ```

- **`removeClass(className)`**: Removes a CSS class.
  ```javascript
  $id('my-element').removeClass('highlight');
  ```

- **`toggleClass(className)`**: Toggles a CSS class on/off.
  ```javascript
  $id('my-element').toggleClass('highlight');
  ```

- **`hasClass(className)`**: Checks if an element has a CSS class.
  ```javascript
  if ($id('my-element').hasClass('highlight')) {
      console.log('Element has the highlight class');
  }
  ```

These methods also work on a `NodeList`:
```javascript
$$('.item').addClass('active'); // Adds 'active' to all .item elements
```

### Visibility
- **`show()`**: Makes an element visible by setting `display: inline-block`.
  ```javascript
  $id('my-element').show();
  ```

- **`hide()`**: Hides an element by setting `display: none`.
  ```javascript
  $id('my-element').hide();
  ```

- **`toggleVisibility()`**: Toggles between visible and hidden.
  ```javascript
  $id('my-element').toggleVisibility();
  ```

**Note**: `show()` uses `inline-block` as the default display. If you need a different `display` style, set it manually:
```javascript
$id('my-element').style.display = 'block';
```

## Event Handling
`macros.js` provides flexible methods for adding and removing event listeners.

### Adding Events
- **`on(event, handler)`**: Adds an event listener to an element or document.
  ```javascript
  $id('my-button').on('click', () => {
      console.log('Button clicked!');
  });
  ```

- **`on(event, selector, handler)`**: Adds a delegated event listener (for elements matching a selector).
  ```javascript
  document.on('click', '.item', (e) => {
      console.log('Item clicked:', e.target);
  });
  ```

This is similar to jQuery’s `$(document).on('click', '.item', handler)`.

- **NodeList Support**:
  ```javascript
  $$('.item').on('click', () => {
      console.log('An item was clicked');
  });
  ```

### Removing Events
- **`off(event, handler)`**: Removes an event listener.
  ```javascript
  const handler = () => console.log('Click');
  $id('my-button').on('click', handler);
  $id('my-button').off('click', handler);
  ```

**Note**: Delegated events can only be removed using the exact handler reference.

### DOM and Load Events
- **`onReady(callback)`**: Executes a callback once the DOM is loaded.
  ```javascript
  onReady(() => {
      console.log('DOM is ready!');
  });
  ```

- **`onLoad(callback)`**: Executes a callback once the DOM and all resources (e.g., stylesheets) are loaded.
  ```javascript
  onLoad(() => {
      console.log('Everything is loaded!');
  });
  ```

## Currency Formatting
The `formatCurrency` function formats a numeric value as currency:
```javascript
const price = window.formatCurrency(1234.56, 'nl-NL', 'EUR');
// Output: "€1,234.56"
```

You can customize the formatting with options:
```javascript
const price = window.formatCurrency(1234.56, 'en-US', 'USD', {
    minimumFractionDigits: 0
});
// Output: "$1,235"
```

## Example Project
Here’s a simple example of an HTML page using `macros.js` to create an interactive list:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>macros.js Example</title>
    <style>
        .item { padding: 10px; border: 1px solid #ccc; margin: 5px; }
        .active { background-color: #f0f0f0; }
        .hidden { display: none; }
    </style>
    <script src="macros.js"></script>
</head>
<body>
    <button id="toggle">Show/Hide</button>
    <ul id="list">
        <li class="item">Item 1 - €10.00</li>
        <li class="item">Item 2 - €20.00</li>
        <li class="item">Item 3 - €30.00</li>
    </ul>
    <script>
        onReady(() => {
            const list = $id('list');
            
            // Add click event to items
            list.$$('.item').on('click', function() {
                this.toggleClass('active');
            });

            // Show/hide list
            $id('toggle').on('click', () => {
                list.toggleVisibility();
            });

            // Format prices
            list.$$('.item').forEach((item, index) => {
                const price = (index + 1) * 10;
                item.textContent = `Item ${index + 1} - ${window.formatCurrency(price)}`;
            });
        });
    </script>
</body>
</html>
```

This example:
- Uses `onReady` to wait for the DOM to load.
- Adds a click event to each `.item` element to toggle the `active` class.
- Adds a button to show/hide the list.
- Formats prices using `formatCurrency`.

## Comparison with jQuery
Here’s a quick comparison to help transition from jQuery to `macros.js`:

| Functionality           | jQuery                          | macros.js                       |
|------------------------|---------------------------------|---------------------------------|
| Select one element      | `$('div')`                     | `$('div')`                     |
| Select multiple         | `$('div')`                     | `$$('div')`                    |
| Select by ID           | `$('#id')`                     | `$id('id')`                    |
| Add class              | `$('#id').addClass('cls')`     | `$id('id').addClass('cls')`    |
| Remove class           | `$('#id').removeClass('cls')`  | `$id('id').removeClass('cls')` |
| Toggle class           | `$('#id').toggleClass('cls')`  | `$id('id').toggleClass('cls')` |
| Show element           | `$('#id').show()`              | `$id('id').show()`             |
| Hide element           | `$('#id').hide()`              | `$id('id').hide()`             |
| Add event              | `$('#id').on('click', fn)`     | `$id('id').on('click', fn)`    |
| Delegated event        | `$(document).on('click', '.item', fn)` | `document.on('click', '.item', fn)` |
| DOM ready              | `$(document).ready(fn)`        | `onReady(fn)`                  |

**Differences**:
- `macros.js` returns native DOM elements (`Element` or `NodeList`), while jQuery returns a jQuery object.
- `macros.js` lacks animations or AJAX functionality, which jQuery provides.
- `macros.js` is much smaller and has no external dependencies.

## Best Practices
1. **Use `onReady` or `onLoad`**: Place scripts at the end of the `<body>` or use `onReady` to ensure the DOM is available.
2. **Chain Methods**: Leverage method chaining for readable code.
   ```javascript
   $id('my-element').addClass('highlight').show();
   ```
3. **Use Delegated Events**: For dynamically added elements, use `document.on('click', '.selector', handler)` to optimize performance.
4. **Minimize DOM Access**: Cache selectors in variables to avoid repeated DOM queries.
   ```javascript
   const button = $id('my-button');
   button.on('click', () => button.toggleClass('active'));
   ```
5. **Validate Selectors**: Ensure selectors are valid to avoid `null` errors.
   ```javascript
   const element = $('div.my-class');
   if (element) {
       element.addClass('active');
   }
   ```

## Conclusion
`macros.js` is a powerful, lightweight tool for modern web development. It offers a familiar, jQuery-like API while leveraging native browser APIs for better performance. By following the guidelines and examples above, you can quickly and efficiently build interactive web applications.
