# Assertions

Examples of asserting the state or behavior of your application in Cypress, for a full reference of commands, go to [docs.cypress.io](https://on.cypress.io/api)

## Implicit Assertions

### [.should()](https://on.cypress.io/should)

To make an assertion about the current subject, use the `.should()` command.

<!-- fiddle Implicit Assertions / .should() - make an assertion about the current subject -->

```html
<table class="table table-bordered assertion-table">
  <thead>
    <tr>
      <th>#</th>
      <th>Column heading</th>
      <th>Column heading</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
    <tr class="success">
      <th scope="row">3</th>
      <td>Column content</td>
      <td>Column content</td>
    </tr>
  </tbody>
</table>
```

```js
cy.get('.assertion-table')
  .find('tbody tr:last')
  .should('have.class', 'success')
  .find('td')
  .first()
  // checking the text of the  element in various ways
  .should('have.text', 'Column content')
  .should('contain', 'Column content')
  .should('have.html', 'Column content')
  // chai-jquery uses "is()" to check if element matches selector
  .should('match', 'td')
  // to match text content against a regular expression
  // first need to invoke jQuery method text()
  // and then match using regular expression
  .invoke('text')
  .should('match', /column content/i)

// a better way to check element's text content against a regular expression
// is to use "cy.contains"
// https://on.cypress.io/contains
cy.get('.assertion-table')
  .find('tbody tr:last')
  // finds first  element with text content matching regular expression
  .contains('td', /column content/i)
  .should('be.visible')
```

Note: find even more examples of matching element's text content in this [FAQ answer](https://on.cypress.io/using-cypress-faq#How-do-I-get-an-element%E2%80%99s-text-contents).

<!-- fiddle-end -->

#### Input elements

When using HTML input elements, use `have.value` assertion.

<!-- fiddle Implicit Assertions / .should() - input elements have value -->

```html
<input
  id="rent"
  type="text"
  pattern="[0-9]+\.*[0-9]*"
  placeholder="e.g 000.00"
  required
/>
```

```js
cy.get('#rent').type('630.00').should('have.value', '630.00')
```

<!-- fiddle-end -->

#### Non-input elements

<!-- fiddle Implicit Assertions / .should() - non-input elements contain text -->

With non-input HTML elements, you can use the `contain` assertion.

```html
<p id="text-example">A brown fox ...</p>
```

```js
cy.get('#text-example').should('contain', 'brown fox')
```

<!-- fiddle-end -->

#### HTML entities

<!-- fiddle Implicit Assertions / .should() - html entities -->

```html
<span id="y-value">&radic;y</span>
```

```js
cy.get('#y-value')
  // use the text value of the HTML entity
  .should('have.html', '√y')
  // "have.html", "have.text", and "contain"
  // assertions work the same with text
  .and('have.text', '√y')
  .and('contain', '√y')
```

Unfortunately, there is no standard built-in JavaScript method for converting HTML entities like `&radic;` into the browser text. Thus the test can encode it itself to use in the assertion.

```js
// a utility for converting HTML entities into text
const encode = (s) => {
  const p = document.createElement('p')
  p.innerHTML = s // encodes
  return p.innerText
}
cy.get('#y-value').should('have.text', encode('&radic;y'))
```

<!-- fiddle-end -->

#### Placeholder attribute

Let's validate the input element's placeholder attribute.

<!-- fiddle Implicit Assertions / .should() - placeholder attribute -->

```html
<input
  type="text"
  id="inputEmail"
  class="form-control"
  placeholder="Email"
/>
```

```js
cy.get('#inputEmail').should('have.attr', 'placeholder', 'Email')
```

<!-- fiddle-end -->

#### Visible element with text

Let's confirm that the page contains a visible element with some text.

<!-- fiddle Implicit Assertions / .should() - visible non-empty text -->

```html
<div id="greeting">Hello, there!</div>
```

```js
// if we know the precise text we are looking for
cy.get('#greeting')
  .should('be.visible')
  .and('have.text', 'Hello, there!')
// if we do not know the text
cy.get('#greeting')
  .should('be.visible')
  .invoke('text')
  .should('be.a', 'string')
  .and('be.not.empty')
```

<!-- fiddle-end -->

#### Visibility of multiple elements

Only some elements should be visible for the assertion `should('be.visible')` to pass.

<!-- fiddle Implicit Assertions / .should() - visibility of multiple elements -->

```html
<ul id="few-elements">
  <li>first</li>
  <li style="display:none">second</li>
  <li>third</li>
</ul>
```

The test passes, even if some elements are invisible.

```js
cy.get('#few-elements li').should('be.visible').and('have.length', 3)
// while the second element is still invisible
cy.contains('#few-elements li', 'second').should('not.be.visible')
// we can use jQuery selector :visible to get just the visible elements
cy.get('#few-elements li:visible').should('have.length', 2)
// we can filter elements to get just the invisible elements
cy.get('#few-elements li')
  .not(':visible')
  .should('have.length', 1)
  .and('have.text', 'second')
```

<!-- fiddle-end -->

#### Elements becoming invisible

Let's checks if a list of elements becomes invisible after some time.

<!-- fiddle Implicit Assertions / .should() - elements becoming invisible -->

```html
<ul id="multiple-elements">
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
<button id="hide-multiple-elements">Hide items</button>
<script>
  document
    .getElementById('hide-multiple-elements')
    .addEventListener('click', function () {
      // we hide the elements after some unknown delay
      setTimeout(
        function () {
          document
            .querySelectorAll('#multiple-elements li')
            .forEach(function (el) {
              el.style.display = 'none'
            })
        },
        // elements disappear after 1 - 2 seconds
        Math.random() * 1000 + 1000,
      )
    })
</script>
```

At first, all elements are visible

```js
cy.get('#multiple-elements li')
  .should('have.length', 3)
  .and('be.visible')
```

The elements become invisible after clicking on the button

```js
cy.get('#hide-multiple-elements').click()
cy.get('#multiple-elements li')
  // the elements still exist in the DOM
  .should('exist')
  .and('have.length', 3)
  // but should not be visible to the user
  .and('not.be.visible')
```

<!-- fiddle-end -->

#### Text matching the regular expression

We can use regular expressions with "match" assertions to confirm part of the text.

<!-- fiddle Implicit Assertions / .should() - text matching the regular expression -->

```html
<div id="a-greeting">Hello, there!</div>
```

```js
cy.get('#a-greeting')
  .invoke('text')
  .should('match', /^Hello/)
// tip: use cy.contains to find element with text
// matching a regular expression
cy.contains('#a-greeting', /^Hello/)
```

<!-- fiddle-end -->

#### OR match using regular expression

If you want to confirm the text matches one string or another, use a regular expression

<!-- fiddle Implicit Assertions / .should() - OR match -->

```html
<div id="or-match">Joe</div>
```

```js
cy.get('#or-match')
  .invoke('text')
  .should('match', /^(Joe|Mary)$/)
// the same can be done using cy.contains command
cy.contains('#or-match', /^(Joe|Mary)$/)
```

<!-- fiddle-end -->

#### Disabled elements

<!-- fiddle Implicit Assertions / .should() - disabled element -->

```html
<input type="text" id="example-input" disabled />
```

```js
cy.get('#example-input')
  .should('be.disabled')
  // let's enable this element from the test
  .invoke('prop', 'disabled', false)
cy.get('#example-input')
  // we can use "enabled" assertion
  .should('be.enabled')
  // or negate the "disabled" assertion
  .and('not.be.disabled')
```

<!-- fiddle-end -->

### [.and()](https://on.cypress.io/and)

To chain multiple assertions together, use the `.and()` command.

<!-- fiddle Implicit Assertions / .and() - chain multiple assertions together -->

```html
<a
  class="assertions-link active"
  href="https://on.cypress.io"
  target="_blank"
  >Cypress Docs</a
>
```

```js
// https://on.cypress.io/and
cy.get('.assertions-link')
  .should('have.class', 'active')
  .and('have.attr', 'href')
  .and('include', 'cypress.io')
```

<!-- fiddle-end -->

Note that all assertions attached to the same command must pass at the same time for the command to succeed.

### Subject

The implicit assertions keep the original subject and pass it to the next command.

<!-- fiddle Implicit Assertions / .should() - keeps the original subject -->

```js
const employee = {
  person: {
    name: {
      first: 'Joe',
      last: 'Smith',
    },
  },
}
cy.wrap(employee)
  .should('have.key', 'person')
  .then((x) => {
    // we are still working with the entire object
    expect(x).to.equal(employee)
  })
```

<!-- fiddle-end -->

Except for several assertions that DO change the subject:

- `have.property` for objects
- `have.attr` for DOM elements
- `have.prop` for DOM elements

as the next tests demonstrate

#### `have.property` assertion

<!-- fiddle Implicit Assertions / .should() - have.property changes the subject -->

```js
const employee = {
  person: {
    name: {
      first: 'Joe',
      last: 'Smith',
    },
  },
}
cy.wrap(employee)
  .should('have.property', 'person')
  .then((x) => {
    // the current subject has been changed to employee.person
    expect(x).to.equal(employee.person)
    expect(x).to.have.key('name')
  })
// Tip: you can use another implicit assertion to check the yielded property
cy.wrap(employee) // full object
  .should('have.property', 'person') // employee.person
  .should('equal', employee.person) // still employee.person
  .and('have.key', 'name') // still employee.person
  // still employee.person because have.key does not change the subject
  .should('equal', employee.person)
```

<!-- fiddle-end -->

#### Multiple properties

If you want to check multiple properties at once we can use `deep.include` assertion. Note we cannot use `deep.equals` in this case, since the object has extra properties we are not interested in.

<!-- fiddle Implicit Assertions / .should() - multiple properties -->

```js
const person = {
  firstName: 'Joe',
  lastName: 'Smith',
  age: 29,
}
cy.wrap(person).should('deep.include', {
  firstName: 'Joe',
  lastName: 'Smith',
})
```

<!-- fiddle-end -->

#### `have.attr` assertion

<!-- fiddle Implicit Assertions / .should() - have.attr changes the subject -->

```html
<div
  data-cy="subject-example"
  style="color: orange; background-color:green;"
>
  Test div
</div>
```

```js
cy.get('[data-cy=subject-example]')
  .should('have.attr', 'style')
  .then((x) => {
    // x is the complete style attribute
    const withoutWhiteSpace = x.replace(/\s/g, '')
    expect(withoutWhiteSpace).to.equal(
      'color:orange;background-color:green;',
    )
  })
// we can remove the whitespace by invoking the method
// on the yielded subject
cy.get('[data-cy=subject-example]') // jQuery element
  .should('have.attr', 'style') // string attribute
  .invoke('replace', /\s/g, '') // string without whitespace
  .should('equal', 'color:orange;background-color:green;')
```

<!-- fiddle-end -->

#### `have.attr` assertion chain

<!-- fiddle Implicit Assertions / .should() - have.attr matching part of the string -->

If we only know a part of the expected attribute, we can first assert the attribute is present, then use an assertion to match its value.

```html
<a id="my-link" href="/some/complex/link-123">My link</a>
```

```js
cy.get('#my-link')
  .should('have.attr', 'href')
  // check if the href attribute includes given string
  .and('include', 'link-')
// we can use a regular expression
cy.get('#my-link')
  .should('have.attr', 'href')
  .and('match', /\/link\-\d+/)
```

<!-- fiddle-end -->

<!-- fiddle Implicit Assertions / .should() - have.prop changes the subject -->

```html
<div class="first second">
  Test div
</div>
```

```js
cy.get('.first')
  .should('have.prop', 'class')
  .then((x) => {
    // x is the class prop
    expect(x).to.equal('first second')
  })
```

<!-- fiddle-end -->

## Explicit Assertions

### expect

To make a BDD assertion about a specified subject, use `expect`.

<!-- fiddle Explicit Assertions / expect - make an assertion about a specified subject -->

```js
expect(true).to.be.true

const o = { foo: 'bar' }
expect(o).to.equal(o)
expect(o).to.deep.equal({ foo: 'bar' })

// matching text using regular expression
expect('FooBar').to.match(/bar$/i)

// check if the response status code is successful
const statusCode = 204
expect(statusCode, 'status code').to.be.within(200, 399)
```

<!-- fiddle-end -->

#### Compare two lists of elements

Let's compare the text content of two lists. We would like to assert that the second list is a subset of the first one. First, we need to get the text from each list, then compare them.

**Tip:** see recipe "Getting Text from List of Elements" to see how to iterate over the list of elements and get their text content.

<!-- fiddle Explicit Assertions / expect - compare two lists -->

```html
<ol id="first">
  <li>Apples</li>
  <li>Oranges</li>
  <li>Melons</li>
  <li>Grapes</li>
</ol>
<ol id="second">
  <li>Grapes</li>
  <li>Oranges</li>
</ol>
```

```js
const firstList = []
const secondList = []
// let's get the first list of strings
cy.get('#first li').each(($li) => {
  firstList.push($li.text())
})
cy.get('#second li')
  .each(($li) => {
    secondList.push($li.text())
  })
  .then(() => {
    // when this callback runs, both lists will be populated
    expect(firstList).to.include.members(secondList)
  })
```

<!-- fiddle-end -->

### assert

To make a TDD assertion about a specified subject, use `assert`.

<!-- fiddle Explicit Assertions / assert - assert shape of an object -->

```js
const person = {
  name: 'Joe',
  age: 20,
}

assert.isObject(person, 'value is object')
```

<!-- fiddle-end -->

## [Should with callback function](https://on.cypress.io/should#Function)

You can write your own complicated checks using `.should(cb)` function if included assertions are not enough. Pass a function to `should()` with any number of explicit assertions within it. The callback function will be retried until it passes all your explicit assertions or times out.

<!-- fiddle Should(cb) / pass your own callback function to should() -->

```html
<div class="assertions-p">
  <p>Some text from first p</p>
  <p>More text from second p</p>
  <p>And even more text from third p</p>
</div>
```

```js
// Pass a function to should that can have any number
// of explicit assertions within it.
// The ".should(cb)" function will be retried
// automatically until it passes all your explicit assertions or times out.
cy.get('.assertions-p')
  .find('p')
  .should(($p) => {
    // https://on.cypress.io/$
    // return an array of texts from all of the p's
    // @ts-ignore TS6133 unused variable
    const texts = $p.map((i, el) => Cypress.$(el).text())

    // jquery map returns jquery object
    // and .get() convert this to simple array
    const paragraphs = texts.get()

    // array should have length of 3
    expect(paragraphs, 'has 3 paragraphs').to.have.length(3)

    // use second argument to expect(...) to provide clear
    // message with each assertion
    expect(
      paragraphs,
      'has expected text in each paragraph',
    ).to.deep.eq([
      'Some text from first p',
      'More text from second p',
      'And even more text from third p',
    ])
  })
```

<!-- fiddle-end -->

#### Partial string match example

Assert that element's class includes `heading-`.

<!-- fiddle Should(cb) / finds element by class name regex -->

```html
<div class="docs-header">
  <div class="main-abc123 heading-xyz987">Introduction</div>
</div>
```

```js
cy.get('.docs-header')
  .find('div')
  // .should(cb) callback function will be retried
  .should(($div) => {
    expect($div).to.have.length(1)

    const className = $div[0].className

    expect(className).to.match(/heading-/)
  })
  // .then(cb) callback is not retried,
  // it either passes or fails
  .then(($div) => {
    expect($div, 'text content').to.have.text('Introduction')
  })
```

<!-- fiddle-end -->

#### Throwing own errors

You can throw any error from the callback function. The callback will be retried, but the assertions will not be shown as nicely in the Command Log UI as Chai assertions.

<!-- fiddle Should(cb) / can throw any error -->

```html
<div class="docs-header-example">
  <div class="heading-top">Top content</div>
</div>
```

```js
cy.get('.docs-header-example')
  .find('div')
  .should(($div) => {
    if ($div.length !== 1) {
      // you can throw your own errors
      throw new Error('Did not find 1 element')
    }

    const className = $div[0].className

    if (!className.match(/heading-/)) {
      throw new Error(
        `Could not find class "heading-" in ${className}`,
      )
    }
  })
```

<!-- fiddle-end -->

#### Dynamic text example

We [strongly recommend that your tests are deterministic](https://on.cypress.io/conditional-testing). But sometimes you might need to match text between two elements, and you do not know what that text should be. Save the value from the first element, then compare it from a `should(cb)` callback.

<!-- fiddle Should(cb) / matches unknown text between two elements -->

```html
<div class="two-elements">
  <div class="first">Foo Bar</div>
  <div class="second">foo b a r</div>
</div>
```

```js
/**
 * Text from the first element.
 * @type {string}
 */
let text

/**
 * Normalizes passed text,
 * useful before comparing text with spaces and different capitalization.
 * @param {string} s Text to normalize
 */
const normalizeText = (s) => s.replace(/\s/g, '').toLowerCase()

cy.get('.two-elements')
  .find('.first')
  .then(($first) => {
    // save text from the first element
    text = normalizeText($first.text())
  })

cy.get('.two-elements')
  .find('.second')
  .should(($div) => {
    // we can massage text before comparing
    const secondText = normalizeText($div.text())

    expect(secondText, 'second text').to.equal(text)
  })
```

<!-- fiddle-end -->

#### Retrying should callback

Remember that Cypress only [retries the very last command](https://on.cypress.io/retry-ability), if it allows retrying. If you need to perform additional steps before running an assertion, you can use `.should(callbackFn)` to retry multiple operations.

<!-- fiddle Should(cb) / retries the should callback until assertions pass -->

```html
<div class="random-number-example">
  Random number: <span id="random-number">🎁</span>
</div>
<script>
  const el = document.getElementById('random-number')
  setTimeout(function () {
    el.innerText = Math.floor(Math.random() * 10 + 1)
  }, 1500)
</script>
```

```js
cy.get('#random-number').should(($div) => {
  const n = parseFloat($div.text())

  expect(n).to.be.gte(1).and.be.lte(10)
})
```

<!-- fiddle-end -->

## Multiple assertions

If you attach multiple assertions to the same command, all assertions must pass at once. For example, here is a test that shows how to correctly check the disappearing element.

<!-- fiddle Multiple assertions / split the command -->

```html
<div style="display: none" id="loading">Loading ...</div>
<button id="load-something">Load</button>
<script>
  document
    .getElementById('load-something')
    .addEventListener('click', function () {
      const loadingElement = document.getElementById('loading')
      // first show the loading element
      setTimeout(function showLoading() {
        loadingElement.style.display = 'block'
      }, 1500)
      // then hide the loading element
      setTimeout(function hideLoading() {
        loadingElement.style.display = 'none'
      }, 2500)
    })
</script>
```

The command below fails because the element cannot be visible AND invisible at the same time:

```js
// ⛔️ DOES NOT WORK
// cy.get('#load-something').click()
// cy.get('#loading').should('be.visible').and('not.be.visible')
```

Instead split the assertions to have separate command to re-query the element and pass one by one The first command asserts the loading element is visible, the second command gets the element again and asserts the element is invisible:

```js
// ✅ THE CORRECT WAY
cy.get('#load-something').click()
cy.get('#loading').should('be.visible')
cy.get('#loading').should('not.be.visible')
```

It is ok to add multiple assertions that can be true at the same time:

```js
cy.get('#loading').should('exist').and('have.text', 'Loading ...')
```

<!-- fiddle-end -->

## Comparing arrays

Whenever you assert arrays and other objects, you probably mean to assert the values inside, and not the references.

<!-- fiddle Array assertions -->

```js
const arr = ['Apples', 'Bananas', 'Grapes']
// assert that cy.wrap yields the same array reference
// as we passed into it
cy.wrap(arr).should('equal', arr)
// assert the yielded array has the expected items inside
cy.wrap(arr)
  .invoke('reverse')
  .should('deep.equal', ['Grapes', 'Bananas', 'Apples'])
```

<!-- fiddle-end -->

## Adding assertions

### Multiple attributes

The built-in Chai assertion `have.attr` only confirms a single attribute. What if we want to confirm multiple attributes? We could write it like this:

<!-- fiddle Adding assertions / multiple assertions -->

```html
<a id="about-page" href="/about" target="_blank">About</a>
```

```js
cy.contains('a', 'About').should((el) => {
  // we do not care about ID attribute
  expect(el).to.have.attr('href', '/about')
  expect(el).to.have.attr('target', '_blank')
})
```

<!-- fiddle-end -->

### Custom Chai assertion

Alternatively, we can add a custom Chai assertion to our global `chai` object.

<!-- fiddle Adding assertions / adding custom assertion -->

```html
<a id="about-page" href="/about" target="_blank">About</a>
```

```js
// add custom Chai assertion to confirm multiple attributes
chai.use((_chai, utils) => {
  // use "function" syntax to make sure when Chai
  // calls it, the "this" object points at Chai

  function assertAttributes(attributes) {
    Object.keys(attributes).forEach((attr) => {
      const value = this._obj.attr(attr)
      const expectedValue = attributes[attr]
      this.assert(
        value === expectedValue,
        `expected to find attribute **${attr}: ${expectedValue}**, found **${value}**`,
      )
    })
  }
  _chai.Assertion.addMethod('attributes', assertAttributes)
})

// now let's use our custom Chai assertion
// to confirm multiple element attributes at once
cy.contains('a', 'About').should('have.attributes', {
  href: '/about',
  target: '_blank',
  // but we do not care about "id" attribute
})
```

<!-- fiddle-end -->
