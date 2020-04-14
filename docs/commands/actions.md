# Actions

Examples of actions being performed on DOM elements in Cypress, for a full reference of commands, go to [docs.cypress.io](https://on.cypress.io/api)

## [.type()](https://on.cypress.io/type)

To type into a DOM element, use the `.type()` command.

<!-- fiddle type -->

```html
<form>
  <div class="form-group">
    <label for="email1">Email address</label>
    <input
      type="email"
      class="form-control action-email"
      id="email1"
      placeholder="Email"
    />
  </div>
  <div class="form-group">
    <label>Disabled Textarea</label>
    <textarea
      class="form-control action-disabled"
      disabled="disabled"
    ></textarea>
  </div>
</form>
```

```js
cy.get('.action-email')
  .type('fake@email.com')
  .should('have.value', 'fake@email.com')

  // .type() with special character sequences
  .type('{leftarrow}{rightarrow}{uparrow}{downarrow}')
  .type('{del}{selectall}{backspace}')

  // .type() with key modifiers
  .type('{alt}{option}') //these are equivalent
  .type('{ctrl}{control}') //these are equivalent
  .type('{meta}{command}{cmd}') //these are equivalent
  .type('{shift}')

  // Delay each keypress by 0.1 sec
  .type('slow.typing@email.com', { delay: 100 })
  .should('have.value', 'slow.typing@email.com')

cy.get('.action-disabled')
  // Ignore error checking prior to type
  // like whether the input is visible or disabled
  .type('disabled error checking', { force: true })
  .should('have.value', 'disabled error checking')
```

<!-- fiddle-end -->

## [.focus()](https://on.cypress.io/focus)

To focus on a DOM element, use the `.focus()` command.

<script>
  // scripts copied from
  // https://github.com/cypress-io/cypress-example-kitchensink/blob/master/app/assets/js/scripts.js
  // listen to focus to demonstrate logic on focus command
  $('.action-focus').on('focus', (e) => {
    $(e.currentTarget).addClass('focus')
    $(e.currentTarget).prev().css('color', 'orange')
  })
</script>

<!-- fiddle focus -->

```html
<form>
  <div class="form-group">
    <label for="password1">Password</label>
    <input
      type="password"
      class="form-control action-focus"
      id="password1"
      placeholder="Password"
    />
  </div>
</form>
<script>
  // listen to focus to demonstrate logic on focus command
  $('.action-focus').on('focus', (e) => {
    $(e.currentTarget).addClass('focus')
    $(e.currentTarget).prev().css('color', 'orange')
  })
</script>
```

```js
cy.get('.action-focus')
  .focus()
  .should('have.class', 'focus')
  .prev()
  .should('have.attr', 'style', 'color: orange;')
```

<!-- fiddle-end -->

## [.blur()](https://on.cypress.io/blur)

To blur on a DOM element, use the `.blur()` command.

<script>
  // listen to blur to demonstrate logic on blur command
  $('.action-blur').on('blur', (e) => {
    $(e.currentTarget).addClass('error')
    $(e.currentTarget).prev().css('color', 'red')
  })
</script>

<!-- fiddle blur -->

```html
<form>
  <div class="form-group">
    <label for="fullName1">Full Name</label>
    <input
      type="text"
      class="form-control action-blur"
      id="fullName1"
      placeholder="Enter your name"
    />
  </div>
</form>
<script>
  // listen to blur to demonstrate logic on blur command
  $('.action-blur').on('blur', (e) => {
    $(e.currentTarget).addClass('error')
    $(e.currentTarget).prev().css('color', 'red')
  })
</script>
```

```js
cy.get('.action-blur')
  .type('About to blur')
  .blur()
  .should('have.class', 'error')
  .prev()
  .should('have.attr', 'style', 'color: red;')
```

<!-- fiddle-end -->

## [.clear()](https://on.cypress.io/clear)

To clear on a DOM element, use the `.clear()` command.

```html
<form>
  <div class="form-group">
    <label for="description">Describe:</label>
    <input type="text" class="form-control action-clear" id="description" />
  </div>
</form>
```

```js
cy.get('.action-clear')
  .type('Clear this text')
  .should('have.value', 'Clear this text')
  .clear()
  .should('have.value', '')
```

## [.submit()](https://on.cypress.io/submit)

To submit a form, use the `cy.submit()` command.

```html
<form class="action-form">
  <div class="form-group">
    <label for="couponCode1" val="HALFOFF">Coupon Code</label>
    <input type="text" class="form-control" id="couponCode1" />
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

```js
cy.get('.action-form').find('[type="text"]').type('HALFOFF')
cy.get('.action-form')
  .submit()
  .next()
  .should('contain', 'Your form has been submitted!')
```