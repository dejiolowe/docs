---
layout: page
weight: 50
title: Using Handlebars
group: api-v3
navigation:
  show: true
seo:
  title: Using Handlebars
  override: true
  description:
---

## Handlebars overview

[Handlebars.js syntax](https://handlebarsjs.com/) provides a simple, powerful way to include dynamic content directly in email templates. Handlebars.js syntax allows all of this dynamic templating to occur outside of code, meaning changes are done quickly in the template, with no update to a code base required.

This page uses examples from the [dynamic-template section of our email templates GitHub repo](https://github.com/sendgrid/email-templates/tree/master/dynamic-templates).

<call-out>

For our API reference, see [Mail Send with Dynamic Transactional Templates](https://sendgrid.api-docs.io/v3.0/transactional-templates).

</call-out>

## Personalizing email with Handlebars

We do not support full Handlebars.js functionality. Currently, dynamic templates support the following helpers:

- [Substitution](#substitution)
- [Conditional statements](#conditional-statements)
- [Iterations](#iterations)

For a full helper reference, see the [Handlebar.js reference](#handlebarjs-reference) on this page.

## Use cases

Here are example use cases listed with the Handlebars.js helpers used to build the example templates.

### Receipts

This [example receipt template](https://github.com/sendgrid/email-templates/tree/master/dynamic-templates/receipt) uses the following heplers:

- [Substitution](#substitution)
- [Conditional statements](#conditional-statements)
- [Iterations](#iterations)

### Password reset

This [example transactional template](https://github.com/sendgrid/email-templates/tree/master/dynamic-templates/transactional-actions) uses the following helpers:

- [Substitution](#substitution)

### Multiple languages

This is an [example template that lets you have content in multiple languages](https://github.com/sendgrid/email-templates/tree/master/dynamic-templates/different-languages), and it uses the following helpers:

- [Conditional statements](#conditional-statements) - `if/else`

### Newsletter

This [example newsletter template](https://github.com/sendgrid/email-templates/tree/master/dynamic-templates/newsletter) uses the following helpers:

- [Substitution](#substitution)
- [Iterations](#iterations)

### Advertisement

This is an [example template that is advertising items on sale](https://github.com/sendgrid/email-templates/tree/master/dynamic-templates/special-sale), and it uses the following helpers:

- [Substitution](#substitution)
- [Conditional statements](#conditional-statements) - `if/else`
- [Iterations](#iterations)

## Handlebar.js reference

This reference provides sample code blocks for each helper, including HTML email snippets and JSON test data.

### Substitution

Our templates support the following substitutions:

- [Basic replacement](#basic-replacement)
- [Deep object replacement](#deep-object-replacement)
- [Object failure](#object-failure)
- [Replacement with HTML](#replacement-with-html)
- [Uppercase](#uppercase)
- [formatDate](#formatdate)
- [Insert](#insert)

#### Basic replacement

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>Hello {{ firstName }}</p>
```

```json
// Test data
{ "firstName": "Ben" }
```

```html
<!-- Resulting HTML !-->
<p>Hello Ben</p>
```

</code-group>

#### Deep object replacement

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>Hello {{user.profile.firstName}}</p>
```

```json
// Test data
{
  "user": {
    "profile": {
      "firstName": "Ben"
    }
  }
}
```

```html
<!-- Resulting HTML -->
<p>Hello Ben</p>
```

</code-group>

#### Object failure

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>Hello {{user.profile.firstName}}</p>
```

```json
// Test data
{
  "user": {
    "orderHistory": [
      {
        "date": "2/1/2018",
        "item": "shoes"
      },
      {
        "date": "1/4/2017",
        "item": "hat"
      }
    ]
  }
}
```

```html
<!-- Resulting HTML -->
<p>Hello</p>
```

</code-group>

#### Replacement with HTML

<call-out type="warning">

If you include the characters `'`, `"` or `&` in a subject line replacement be sure to use three brackets like below.

</call-out>

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<!-- Per Handlebars' documentation: If you don't want Handlebars to escape a value, use the "triple-stash", {{{ -->
<p>Hello {{{firstName}}}</p>
```

```json
// Test data
{ "firstName": "<strong>Ben</strong>" }
```

```html
<!-- Resulting HTML -->
<p>Hello <strong>Ben</strong></p>
```

</code-group>

#### Uppercase

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>Hello {{uppercase firstName}}</p>
```

```json
// Test data
{ "firstName": "<strong>Ben</strong>" }
```

```html
<!-- Resulting HTML -->
<p>Hello <strong>BEN</strong></p>
```

</code-group>

#### formatDate

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template without timezone -->
<p>Join us {{formatDate ts format}}</p>

<!-- Template with timezone -->
<p>Join us {{formatDate ts format \"-0000\"}}</p>
```

```json
// Test data
{
  "ts": "2020-01-15 19:31:05Z",
  "format": "yyyy-mm-dd"
}
```

```html
<!-- Resulting HTML without timezone-->
<p>Join us 2020-01-15</p>

<!-- Resulting HTML with timezone-->
<p>Join us 2020-01-15</p>
```

</code-group>

#### Insert

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Insert with a default value -->
<p>Hello {{insert name "default=Customer"}}! Thank you for contacting us about {{insert businessName "your business"}}.</p>
```

```json
// Test data with all values
{
   "name": "Ben",
   "businessName": "Twilio SendGrid"
}

// Test data with missing value
{
  "name": "Ben"
}
```

```html
<!-- Resulting HTML with all values -->
<p>Hello Ben! Thank you for contacting us about Twilio SendGrid.</p>

<!-- Resulting HTML with missing value and a default-->
<p>Hello Ben! Thank you for contacting us about your business.</p>
```

</code-group>

### Conditional statements

Our templates support the following conditionals:

- [Basic If, Else, Else If](#basic-if-else-else-if)
- [If with a root](#if-with-a-root)
- [Unless](#unless)
- [greaterThan](#greaterthan)
- [lessThan](#lessthan)
- [Equals](#equals)
- [notEquals](#notequals)
- [And](#and)
- [Or](#or)
- [Length](#length)
- [isBefore](#isbefore)
- [isAfter](#isafter)

#### Basic If, Else, Else If

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
{{#if user.profile.male}}
   <p>Dear Sir</p>
{{else if user.profile.female}}
   <p>Dear Madame</p>
{{else}}
   <p>Dear Customer</p>
{{/if}}
```

```json
// Test data one
{
   "user":{
      "profile":{
         "male":true
      }
   }
}

// Test data two
{
   "user":{
      "profile":{
         "female":true
      }
   }
}

// Test data three
{
   "user":{
      "profile":{

      }
   }
}
```

```html
<!-- Resulting HTML from test data one -->
<p>Dear Sir</p>

<!-- Resulting HTML from test data two -->
<p>Dear Madame</p>

<!-- Resulting HTML from test data three -->
<p>Dear Customer</p>
```

</code-group>

#### If with a root

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
{{#if user.suspended}}
   <p>Warning! Your account is suspended, please call: {{@root.supportPhone}}</p>
{{/if}}
```

```json
// Test data
{
  "user": {
    "suspended": true
  },
  "supportPhone": "1-800-555-5555"
}
```

```html
<!-- Resulting HTML -->
<p>Warning! Your account is suspended, please call: 1-800-555-5555</p>
```

</code-group>

#### Unless

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
{{#unless user.active}}
   <p>Warning! Your account is suspended, please call: {{@root.supportPhone}}</p>
{{/unless}}
```

```json
// Test data
{
  "user": {
    "active": false
  },
  "supportPhone": "1-800-555-5555"
}
```

```html
<!-- Resulting HTML -->
<p>Warning! Your account is suspended, please call: 1800-555-5555</p>
```

</code-group>

#### greaterThan

##### Basic greaterThan

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#greaterThan scoreOne scoreTwo}}
    Congratulations, you have the high score today!
{{/greaterThan}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "scoreOne": 100,
  "scoreTwo": 78
}

// Test data two
{
  "scoreOne": 55,
  "scoreTwo": 78
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! Congratulations, you have the high score today! Thanks for playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! Thanks for playing.
</p>
```

</code-group>

##### greaterThan with else

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#greaterThan scoreOne scoreTwo}}
    Congratulations, you have the high score today!
{{else}}
    You were close, but you didn't get the high score today.
{{/greaterThan}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "scoreOne": 100,
  "scoreTwo": 78
}

// Test data two
{
  "scoreOne": 55,
  "scoreTwo": 78
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! Congratulations, you have the high score today! Thanks for playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! You were close, but you didn't get the high score today. Thanks for
  playing.
</p>
```

</code-group>

#### lessThan

##### Basic lessThan

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#lessThan scoreOne scoreTwo}}
    You were close, but you didn't get the high score today.
{{/lessThan}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "scoreOne": 55,
  "scoreTwo": 78
}

// Test data two
{
  "scoreOne": 100,
  "scoreTwo": 78
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! You were close, but you didn't get the high score today. Thanks for
  playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! Thanks for playing.
</p>
```

</code-group>

##### lessThan with else

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#lessThan scoreOne scoreTwo}}
    You were close, but you didn't get the high score today.
{{else}}
    Congratulations, you have the high score today!
{{/lessThan}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "scoreOne": 55,
  "scoreTwo": 78
}

// Test data two
{
  "scoreOne": 100,
  "scoreTwo": 78
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! You were close, but you didn't get the high score today. Thanks for
  playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! Congratulations, you have the high score today! Thanks for playing.
</p>
```

</code-group>

#### Equals

The `equals` comparison can check for equality between two values of the same data type. The `equals` helper will also attempt to coerce data types to make a comparison of values independent of their data type. For example, `{{#equals 3 "3"}}` will evaluate to `true`.

When checking for truthiness, be aware that empty strings, zero integers, and zero floating poing numbers evaluate to `false`. Non-empty strings, non-zero integers, and non-zero floating point numbers, including negative numbers, evaluate to `true`.

##### Basic equals

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#equals customerCode winningCode}}
    You have a winning code.
{{/equals}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "customerCode": 289199,
  "winningCode": 289199
}

// Test data two
{
  "customerCode": 167320,
  "winningCode": 289199
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! You have a winning code. Thanks for playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! Thanks for playing.
</p>
```

</code-group>

##### Equals with else

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#equals customerCode winningCode}}
    You have a winning code.
{{else}}
    You do not have a winning code.
{{/equals}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "customerCode": 289199,
  "winningCode": 289199
}

// Test data two
{
  "customerCode": 167320,
  "winningCode": 289199
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! You have a winning code. Thanks for playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! You do not have a winning code. Thanks for playing.
</p>
```

</code-group>

#### notEquals

The `notEquals` comparison can check for equality between two values of the same data type. The `notEquals` helper will also attempt to coerce data types to make a comparison of values independent of their data type. For example, {{#equals 3 "3"}} will return `false`.

When checking for truthiness, be aware that empty strings, zero integers, and zero floating poing numbers evaluate to `false`. Non-empty strings, non-zero integers, and non-zero floating point numbers, including negative numbers, evaluate to `true`.

##### Basic notEquals

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#notEquals customerCode winningCode}}
    You have a winning code.
{{/notEquals}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "customerCode": 289199,
  "winningCode": 289199
}

// Test data two
{
  "customerCode": 167320,
  "winningCode": 289199
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! You have a winning code. Thanks for playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! Thanks for playing.
</p>
```

</code-group>

##### notEquals with else

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#notEquals customerCode winningCode}}
    You have a winning code.
{{else}}
    You do not have a winning code.
{{/notEquals}}
 Thanks for playing.
</p>
```

```json
// Test data one
{
  "customerCode": 289199,
  "winningCode": 289199
}

// Test data two
{
  "customerCode": 167320,
  "winningCode": 289199
}
```

```html
<!-- Resulting HTML from test data one-->
<p>
  Hello Ben! You have a winning code. Thanks for playing.
</p>

<!-- Resulting HTML from test data two-->
<p>
  Hello Ben! You do not have a winning code. Thanks for playing.
</p>
```

</code-group>

#### And

When checking for truthiness, be aware that empty strings, zero integers, and zero floating poing numbers evaluate to `false`. Non-empty strings, non-zero integers, and non-zero floating point numbers, including negative numbers, evaluate to `true`.

##### And without else

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#and favoriteFood favoriteDrink}}
   Thank you for letting us know your dining preferences.
{{/and}}.
 We look forward to sending you more delicious recipes.</p>
```

```json
// Test data one
{
  "favoriteFood": "Pasta",
  "favoriteDrink": ""
}

// Test data two
{
  "favoriteFood": "Pasta",
  "favoriteDrink": "Coffee"
}
```

```html
<!-- Resulting HTML from test data one -->
<p>Hi Ben! We look forward to sending you more delicious recipes.</p>

<!-- Resulting HTML from test data two -->
<p>
  Hi Ben! Thank you for letting us know your dining preferences. We look forward
  to sending you more delicious recipes.
</p>
```

</code-group>

##### And with else

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#and favoriteFood favoriteDrink}}
   Thank you for letting us know your dining preferences.
{{else}}
   If you finish filling out your dining preferences survey, we can deliver you recipes we think you'll be most interested in.
{{/and}}.
 We look forward to sending you more delicious recipes.</p>
```

```json
// Test data one
{
  "favoriteFood": "Pasta",
  "favoriteDrink": ""
}

// Test data two
{
  "favoriteFood": "Pasta",
  "favoriteDrink": "Coffee"
}
```

```html
<!-- Resulting HTML from test data one -->
<p>
  Hi Ben! If you finish filling out your dining preferences survey, we can
  deliver you recipes we think you'll be most interested in. We look forward to
  sending you more delicious recipes.
</p>

<!-- Resulting HTML from test data two -->
<p>
  Hi Ben! Thank you for letting us know your dining preferences. We look forward
  to sending you more delicious recipes.
</p>
```

</code-group>

#### Or

When checking for truthiness, be aware that empty strings, zero integers, and zero floating poing numbers evaluate to `false`. Non-empty strings, non-zero integers, and non-zero floating point numbers, including negative numbers, evaluate to `true`.

##### Basic or

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#or isRunner isCyclist}}
   We think you might enjoy a map of trails in your area.
{{/or}}.
 Have a great day.
</p>
```

```json
// Test data one
{
  "isRunner": true,
  "isCyclist": false
}

// Test data two
{
  "isRunner": false,
  "isCyclist": false
}
// Test data three
{
  "isRunner": false,
  "isCyclist": true
}
```

```html
<!-- Resulting HTML from test data one -->
<p>
  Hi Ben! We think you might enjoy a map of trails in your area. You can find
  the map attached to this email. Have a great day.
</p>

<!-- Resulting HTML from test data two -->
<p>
  Hi Ben! Have a great day.
</p>

<!-- Resulting HTML from test data three -->
<p>
  Hi Ben! We think you might enjoy a map of trails in your area. You can find
  the map attached to this email. Have a great day.
</p>
```

</code-group>

##### Or with else

<code-group langs="Hanblebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>
Hello Ben!
{{#or isRunner isCyclist}}
   We think you might enjoy a map of trails in your area. You can find the map attached to this email.
{{else}}
   We'd love to know more about the outdoor activities you enjoy. The survey linked below will take only a minute to fill out.
{{/or}}.
 Have a great day.
</p>
```

```json
// Test data one
{
  "isRunner": true,
  "isCyclist": false
}

// Test data two
{
  "isRunner": false,
  "isCyclist": false
}
// Test data three
{
  "isRunner": false,
  "isCyclist": true
}
```

```html
<!-- Resulting HTML from test data one -->
<p>
  Hi Ben! We think you might enjoy a map of trails in your area. You can find
  the map attached to this email. Have a great day.
</p>

<!-- Resulting HTML from test data two -->
<p>
  Hi Ben! We'd love to know more about the outdoor activities you enjoy. The
  survey linked below will take only a minute to fill out. Have a great day.
</p>

<!-- Resulting HTML from test data three -->
<p>
  Hi Ben! We think you might enjoy a map of trails in your area. You can find
  the map attached to this email. Have a great day.
</p>
```

</code-group>

#### Length

The length helper will return the number of characters in a given string or array. For non-string and non-array values, length will return 0. Length can be useful in combination with other helpers as shown with greaterThan in the following example.

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Templates -->
<p>
Hello Ben!
{{#greaterThan 0 length cartItems}}
 It looks like you still have some items in your shopping cart. Sign back in to continue checking out at any time.
{{else}}
 Thanks for browsing our site. We hope you'll come back soon.
{{/greaterThan}}
</p>
```

```json
// Test data one
{
  "cartItems": ["raft", "water bottle", "sleeping bag"]
}

// Test data two
{
  "cartItems": []
}
```

```html
<!-- Resulting HTML with test data one-->
<p>
  Hello Ben! It looks like you still have some items in your shopping cart. Sign
  back in to continue checking out at any time.
</p>

<!-- Resulting HTML with test data two-->
<p>
  Hello Ben! Thanks for browsing our site. We hope you'll come back soon.
</p>
```

</code-group>

#### isBefore

The isBefore helper will compare two epochs (datetime values) and render a block of HTML if the first value is earlier (occurs before) the second value.

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>Hello Ben!
{{#isBefore requestedTime deadline}}
   You've successfully reserved a time with us. We'll see you soon.
{{else}}
   The time you requested is after the deadline. Please login to select another time.
{{/isBefore}}</p>
```

```json
// Test data one
{
  "requestedTime": "2020-01-15 10:30:05Z",
  "deadline": "2020-01-10 10:30:05Z"
}

// Test data two
{
  "requestedTime": "2020-01-15 10:30:05Z",
  "deadline": "2020-01-10 10:30:05Z"
}
```

```html
<!-- Resulting HTML from test data one -->
<p>
  Hello Ben! The time you requested is after the deadline. Please login to
  select another time.
</p>

<!-- Resulting HTML from test data two -->
<p>
  Hello Ben! You've successfully reserved a time with us. We'll see you soon.
</p>
```

</code-group>

#### isAfter

The isAfter helper will compare two epochs (datetime values) and render a block of HTML if the first value is later (occurs after) the second value.

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<p>Hello Ben!
{{#isAfter requestedTime deadline}}
   The time you requested is after the deadline. Please login to select another time.
{{else}}
   You've successfully reserved a time with us. We'll see you soon.
{{/isAfter}}</p>
```

```json
// Test data one
{
  "requestedTime": "2020-01-15 10:30:05Z",
  "deadline": "2020-01-10 10:30:05Z"
}

// Test data two
{
  "requestedTime": "2020-01-15 10:30:05Z",
  "deadline": "2020-01-10 10:30:05Z"
}
```

```html
<!-- Resulting HTML from test data one -->
<p>
  Hello Ben! You've successfully reserved a time with us. We'll see you soon.
</p>

<!-- Resulting HTML from test data two -->
<p>
  Hello Ben! The time you requested is after the deadline. Please login to
  select another time.
</p>
```

</code-group>

### Iterations

You can loop or iterate over data using the `{{#each }}` helper function to build lists and perform other useful templating actions.

#### Basic Iterator with each

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
<ol>
  {{#each user.orderHistory}}
   <li>You ordered: {{this.item}} on: {{this.date}}</li>
  {{/each}}
</ol>
```

```json
// Test data
{
  "user": {
    "orderHistory": [
      {
        "date": "2/1/2018",
        "item": "shoes"
      },
      {
        "date": "1/4/2017",
        "item": "hat"
      }
    ]
  }
}
```

```html
<!-- Resulting HTML -->
<ol>
  <li>You ordered: shoes on: 2/1/2018</li>
  <li>You ordered: hat on: 1/42017</li>
</ol>
```

</code-group>

### Combined examples

The following examples show you how to combine multiple Handlebars functions to create a truly dynamic template.

- [Dynamic content creation](#dynamic-content-creation)
- [Dynamic content creation with dynamic parts 1](#dynamic-content-creation-with-dynamic-parts-1)
- [Dynamic content creation with dynamic parts 2](#dynamic-content-creation-with-dynamic-parts-2)

#### Dynamic content creation

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
{{#each user.story}}
   {{#if this.male}}
      <p>{{this.date}}</p>
   {{else if this.female}}
      <p>{{this.item}}</p>
   {{/if}}
{{/each}}
```

```json
// Test data
{
  "user": {
    "story": [
      {
        "male": true,
        "date": "2/1/2018",
        "item": "shoes"
      },
      {
        "male": true,
        "date": "1/4/2017",
        "item": "hat"
      },
      {
        "female": true,
        "date": "1/1/2016",
        "item": "shirt"
      }
    ]
  }
}
```

```html
<!-- Resulting HTML -->
<p>2/1/2018</p>
<p>1/4/2017</p>
<p>shirt</p>
```

</code-group>

#### Dynamic content creation with dynamic parts 1

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
{{#each user.story}}
   {{#if this.male}}
      {{#if this.date}}
         <p>{{this.date}}</p>
      {{/if}}
      {{#if this.item}}
         <p>{{this.item}}</p>
      {{/if}}
   {{else if this.female}}
      {{#if this.date}}
         <p>{{this.date}}</p>
      {{/if}}
      {{#if this.item}}
         <p>{{this.item}}</p>
      {{/if}}
   {{/if}}
{{/each}}
```

```json
// Test data
{
  "user": {
    "story": [
      {
        "male": true,
        "date": "2/1/2018",
        "item": "shoes"
      },
      {
        "male": true,
        "date": "1/4/2017"
      },
      {
        "female": true,
        "item": "shirt"
      }
    ]
  }
}
```

```html
<!-- Resulting HTML -->
<p>2/1/2018</p>
<p>shoes</p>
<p>1/4/2017</p>
<p>shirt</p>
```

</code-group>

#### Dynamic content creation with dynamic parts 2

<code-group langs="Handlebars, JSON, HTML">

```handlebars
<!-- Template -->
{{#if people}}
   <p>People:</p>
   {{#each people}}
      <p>{{this.name}}</p>
   {{/each}}
{{/if}}
```

```json
// Test data
{
  "people": [{ "name": "Bob" }, { "name": "Sally" }]
}
```

```html
<!-- Resulting HTML -->
<p>People:</p>
<p>Bob</p>
<p>Sally</p>
```

</code-group>

## Additional Resources

- [Sending Email with Dynamic Transactional Templates]({{root_url}}/ui/sending-email/how-to-send-an-email-with-dynamic-transactional-templates/)
- [Create and edit Dynamic Transactional Templates]({{root_url}}/ui/sending-email/create-and-edit-transactional-templates/)
- [Dynamic Templates API](https://sendgrid.api-docs.io/v3.0/transactional-templates)
- [How to send an email with dynamic templates]({{root_url}}/ui/sending-email/how-to-send-an-email-with-dynamic-transactional-templates/)
