# Handboek voor macros.js

Dit handboek biedt een uitgebreide uitleg over het gebruik van `macros.js` in je webprojecten. `macros.js` is een lichtgewicht JavaScript-bibliotheek die veelgebruikte DOM-manipulaties en event-handling vereenvoudigt, vergelijkbaar met jQuery, maar met een moderne en compacte aanpak. Het is ontworpen om direct in de browser te werken zonder afhankelijkheden.

## Inhoudsopgave
1. [Introductie](#introductie)
2. [Installatie](#installatie)
3. [DOM-selectors](#dom-selectors)
4. [DOM-manipulatie](#dom-manipulatie)
5. [Event-handling](#event-handling)
6. [Valuta-opmaak](#valuta-opmaak)
7. [Voorbeeldproject](#voorbeeldproject)
8. [Vergelijking met jQuery](#vergelijking-met-jquery)
9. [Best practices](#best-practices)

## Introductie
`macros.js` biedt een verzameling hulpfuncties en methoden om DOM-elementen te selecteren, manipuleren en gebeurtenissen te beheren. Het is een moderne vervanging voor jQuery, met een focus op eenvoud en prestaties. Het ondersteunt:
- Verkorte DOM-selectors (`$`, `$$`, `$id`)
- Ketenbare methoden voor DOM-manipulatie (`addClass`, `removeClass`, `toggleClass`, `show`, `hide`, `toggleVisibility`)
- Flexibele event-handling (`on`, `off`)
- Valuta-opmaak (`formatCurrency`)

In tegenstelling tot jQuery is `macros.js` compacter en maakt het gebruik van moderne JavaScript-API's zoals `querySelector` en `classList`.

## Installatie
Voeg `macros.js` toe aan je project door het script in je HTML-bestand op te nemen:

```html
<script src="macros.js"></script>
```

Plaats het `<script>`-tag aan het einde van de `<body>` om ervoor te zorgen dat de DOM volledig is geladen, of gebruik de `onReady`-functie (zie verder).

## DOM-selectors
`macros.js` biedt drie globale functies voor het selecteren van elementen, vergelijkbaar met jQuery:

- **`$`**: Selecteert het eerste element dat overeenkomt met een CSS-selector.
  ```javascript
  const element = $('div.my-class'); // Vergelijkbaar met jQuery $('div.my-class')
  ```

- **`$$`**: Selecteert alle elementen die overeenkomen met een CSS-selector als een `NodeList`.
  ```javascript
  const elements = $$('div.my-class'); // Vergelijkbaar met jQuery $('div.my-class')
  ```

- **`$id`**: Selecteert een element op basis van zijn ID.
  ```javascript
  const element = $id('my-id'); // Vergelijkbaar met jQuery $('#my-id')
  ```

Je kunt ook selectors gebruiken binnen een specifiek element:
```javascript
const parent = $id('parent');
const child = parent.$('.child'); // Selecteert eerste .child binnen #parent
const children = parent.$$('.child'); // Selecteert alle .child binnen #parent
```

## DOM-manipulatie
`macros.js` biedt ketenbare methoden om DOM-elementen te manipuleren, vergelijkbaar met jQuery's ketenbare API.

### Klassebeheer
- **`addClass(className)`**: Voegt een CSS-klasse toe.
  ```javascript
  $id('my-element').addClass('highlight');
  ```

- **`removeClass(className)`**: Verwijdert een CSS-klasse.
  ```javascript
  $id('my-element').removeClass('highlight');
  ```

- **`toggleClass(className)`**: Wisselt een CSS-klasse aan/uit.
  ```javascript
  $id('my-element').toggleClass('highlight');
  ```

- **`hasClass(className)`**: Controleert of een element een CSS-klasse heeft.
  ```javascript
  if ($id('my-element').hasClass('highlight')) {
      console.log('Element heeft de klasse highlight');
  }
  ```

Deze methoden werken ook op een `NodeList`:
```javascript
$$('.item').addClass('active'); // Voegt 'active' toe aan alle .item-elementen
```

### Zichtbaarheid
- **`show()`**: Maakt een element zichtbaar door `display: inline-block` in te stellen.
  ```javascript
  $id('my-element').show();
  ```

- **`hide()`**: Verbergt een element door `display: none` in te stellen.
  ```javascript
  $id('my-element').hide();
  ```

- **`toggleVisibility()`**: Wisselt tussen zichtbaar en verborgen.
  ```javascript
  $id('my-element').toggleVisibility();
  ```

**Opmerking**: `show()` gebruikt `inline-block` als standaardweergave. Als je een andere `display`-stijl nodig hebt, stel je deze handmatig in:
```javascript
$id('my-element').style.display = 'block';
```

## Event-handling
`macros.js` biedt flexibele methoden voor het toevoegen en verwijderen van gebeurtenislisteners.

### Gebe Sohnissen toevoegen
- **`on(event, handler)`**: Voegt een gebeurtenislistener toe aan een element of document.
  ```javascript
  $id('my-button').on('click', () => {
      console.log('Knop geklikt!');
  });
  ```

- **`on(event, selector, handler)`**: Voegt een gedelegeerde gebeurtenislistener toe (voor elementen die overeenkomen met een selector).
  ```javascript
  document.on('click', '.item', (e) => {
      console.log('Item geklikt:', e.target);
  });
  ```

Dit is vergelijkbaar met jQuery's `$(document).on('click', '.item', handler)`.

- **NodeList-ondersteuning**:
  ```javascript
  $$('.item').on('click', () => {
      console.log('Een item is geklikt');
  });
  ```

### Gebeurtenissen verwijderen
- **`off(event, handler)`**: Verwijdert een gebeurtenislistener.
  ```javascript
  const handler = () => console.log('Klik');
  $id('my-button').on('click', handler);
  $id('my-button').off('click', handler);
  ```

**Opmerking**: Gedelegeerde gebeurtenissen kunnen alleen worden verwijderd door de exacte handler-referentie te gebruiken.

### DOM- en laad-gebeurtenissen
- **`onReady(callback)`**: Voert een callback uit zodra de DOM is geladen.
  ```javascript
  onReady(() => {
      console.log('DOM is klaar!');
  });
  ```

- **`onLoad(callback)`**: Voert een callback uit zodra de DOM en alle middelen (zoals stylesheets) zijn geladen.
  ```javascript
  onLoad(() => {
      console.log('Alles is geladen!');
  });
  ```

## Valuta-opmaak
De functie `formatCurrency` formatteert een numerieke waarde als valuta:
```javascript
const prijs = window.formatCurrency(1234.56, 'nl-NL', 'EUR');
// Output: "€ 1.234,56"
```

Je kunt de opmaak aanpassen met opties:
```javascript
const prijs = window.formatCurrency(1234.56, 'en-US', 'USD', {
    minimumFractionDigits: 0
});
// Output: "$1,235"
```

## Voorbeeldproject
Hier is een eenvoudig voorbeeld van een HTML-pagina die `macros.js` gebruikt om een interactieve lijst te maken:

```html
<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <title>macros.js Voorbeeld</title>
    <style>
        .item { padding: 10px; border: 1px solid #ccc; margin: 5px; }
        .active { background-color: #f0f0f0; }
        .hidden { display: none; }
    </style>
    <script src="macros.js"></script>
</head>
<body>
    <button id="toggle">Toon/Verberg</button>
    <ul id="list">
        <li class="item">Item 1 - € 10,00</li>
        <li class="item">Item 2 - € 20,00</li>
        <li class="item">Item 3 - € 30,00</li>
    </ul>
    <script>
        onReady(() => {
            const list = $id('list');
            
            // Voeg klikgebeurtenis toe aan items
            list.$$('.item').on('click', function() {
                this.toggleClass('active');
            });

            // Toon/verberg lijst
            $id('toggle').on('click', () => {
                list.toggleVisibility();
            });

            // Formatteer prijzen
            list.$$('.item').forEach((item, index) => {
                const prijs = (index + 1) * 10;
                item.textContent = `Item ${index + 1} - ${window

.formatCurrency(prijs)}`;
            });
        });
    </script>
</body>
</html>
```

Dit voorbeeld:
- Gebruikt `onReady` om te wachten tot de DOM is geladen.
- Voegt een klikgebeurtenis toe aan elk `.item`-element om de `active`-klasse te wisselen.
- Voegt een knop toe om de lijst te tonen/verbergen.
- Formateert prijzen met `formatCurrency`.

## Vergelijking met jQuery
Hier is een snelle vergelijking om de overgang van jQuery naar `macros.js` te vergemakkelijken:

| Functionaliteit         | jQuery                          | macros.js                       |
|------------------------|---------------------------------|---------------------------------|
| Selecteer één element   | `$('div')`                     | `$('div')`                     |
| Selecteer meerdere     | `$('div')`                     | `$$('div')`                    |
| Selecteer op ID        | `$('#id')`                     | `$id('id')`                    |
| Klasse toevoegen       | `$('#id').addClass('cls')`     | `$id('id').addClass('cls')`    |
| Klasse verwijderen      | `$('#id').removeClass('cls')`  | `$id('id').removeClass('cls')` |
| Klasse wisselen        | `$('#id').toggleClass('cls')`  | `$id('id').toggleClass('cls')` |
| Toon element           | `$('#id').show()`              | `$id('id').show()`             |
| Verberg element        | `$('#id').hide()`              | `$id('id').hide()`             |
| Event toevoegen        | `$('#id').on('click', fn)`     | `$id('id').on('click', fn)`    |
| Gedelegeerd event      | `$(document).on('click', '.item', fn)` | `document.on('click', '.item', fn)` |
| DOM klaar              | `$(document).ready(fn)`        | `onReady(fn)`                  |

**Verschillen**:
- `macros.js` retourneert native DOM-elementen (`Element` of `NodeList`), terwijl jQuery een jQuery-object retourneert.
- `macros.js` heeft geen animaties of AJAX-functionaliteit, wat jQuery wel biedt.
- `macros.js` is veel kleiner en heeft geen externe afhankelijkheden.

## Best practices
1. **Gebruik `onReady` of `onLoad`**: Plaats je scripts aan het einde van de `<body>` of gebruik `onReady` om te zorgen dat de DOM beschikbaar is.
2. **Keten methoden**: Maak gebruik van de ketenbaarheid van methoden zoals `addClass` en `show` voor leesbare code.
   ```javascript
   $id('my-element').addClass('highlight').show();
   ```
3. **Gebruik gedelegeerde events**: Voor dynamisch toegevoegde elementen, gebruik `document.on('click', '.selector', handler)` om prestaties te optimaliseren.
4. **Minimaliseer DOM-toegang**: Cache selectors in variabelen om herhaalde DOM-query's te vermijden.
   ```javascript
   const button = $id('my-button');
   button.on('click', () => button.toggleClass('active'));
   ```
5. **Valideer selectors**: Zorg ervoor dat je selectors correct zijn om `null`-fouten te voorkomen.
   ```javascript
   const element = $('div.my-class');
   if (element) {
       element.addClass('active');
   }
   ```

## Conclusie
`macros.js` is een krachtige, lichtgewicht tool voor moderne webontwikkeling. Het biedt een vertrouwde, jQuery-achtige API, maar maakt gebruik van native browser-API's voor betere prestaties. Door de bovenstaande richtlijnen en voorbeelden te volgen, kun je snel en efficiënt interactieve webapplicaties bouwen.
