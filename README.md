# SEAMS - A CSS Methodology

SEAMS is a CSS methodology that helps you write and organize your CSS better. It’s inspired by many others that exist today, such as [Atomic Design](http://bradfrostweb.com/blog/post/atomic-web-design/), [SMACSS](http://smacss.com/) and [BEM](http://bem.info/), but with more consideration towards themable projects.

With SEAMS you organize your CSS into interchangeable parts that can be reused and modified easily; think car manufacturing and maintenance. Hand-crafted, bespoke CSS has no place here and the ideal is to reuse existing UI as much as possible, instead of creating new pieces of UI for every new feature from scratch.

In this document I’ll give you a rundown of SEAMS so you need to implement it in your project.

## The Basics

SEAMS is an acronym for the categories we use to organize our CSS: Structures, Elements, Augmentations, Modules and Skins.

- Structures are the large blocks of your UI – often appearing across multiple pages – used to place your Modules and Elements in, such as a header or sidebar
- Elements are the foundation of your UI and used to construct Modules and Structures, or left alone to provide basic styling for your UI
- Augmentations are used to change the state of parts of your UI, such as hiding/showing a Module or animating an Element using classes
- Modules are made up of Elements and used to display groups of data and functionality, such as a list of emails or a search bar
- Skins are changes to parts of your UI for aesthetic and rebranding purposes, such as changing the background colour of a Structure or fonts used in a Module

### Structures

Structures are the scaffolding for your UI and are what you would categorize the parts of your UI that are used consistently across your product. Your grid system and product-wide containers, headers, footers, etc. would be created in this category.

Although Structures tend to be used only once per page, if you avoid the use of IDs and other over-qualified selectors, then your Structures will naturally be reusable without extra work.

### Elements

Elements are the basic building blocks of your UI that can be used to build your Structures and Modules.

Although we call this category Elements, we don’t mean that you should be targeting HTML elements with your selectors. Rather you should be adding classes to generic elements and styling those generic classes.

### Augmentations
Augmentations are used to assist you with manipulating parts of your UI using class names. They can be used as helpers too add a transition effect to Elements, or used to hide and unhide modules given a condition.

Augmentations are useful for making sure you don’t fall for bad habits you you want to modify your UI using JavaScript, such as inline styling to make a Module bigger than it is at stock size.

### Modules
Modules are the bread & butter of your UI, used to style most of the interactive parts of your product. We’re talking about lists of todos, new entry forms, modal windows, etc.

Modules are composed of Elements and can be used within Structures or other Modules if you desire.

### Skins
Skins contain the declarations you would modify when changing your UI for the repurposes of theming and rebranding.

We keep these separate from our other styles so that we can work on them efficiently during development and reduce compilation headaches.

## Classes and Selectors

In addition to our categories, we also prescribe a specific way of writing your classes and selectors.

- Do not separate words within a class using hyphens or underscores. These are reserved for prefixing words for certain categories to make them easier to identify, such as Augmentations. Example: `searchbar` is a good name, but not `search-bar` and we prefix Augmentations with `is-`, so a class that hides elements would be `is-hidden`
- The classes of child elements in a Module, including those nested very deep, should start with the class of the parent element used to name the Module. Example: a Module with the class `inbox` would have Elements like `inbox-item`, `inbox-link` and `inbox-readstatus`
- Classes should remain sacred; do not alter or add classes to a Module or its Elements just to make selecting them easier when used in a Structure. Instead, write your selectors by targeting the class of the Structure followed by the class of the Module or Element of that Module. Example: `.content .hero`, `.header .nav-item`
- Don’t get carried away with nesting your selectors. The ideal is one level deep; just because an Element is inside another Element doesn’t mean you have to nest one under the other to select it.
- Do not use ID selectors. They can lead to headaches when trying to overwrite their declarations with Skins or Augmentations due to their higher specificity.
- Avoid using element selectors as much as possible. Use them to provide a very minimal base for your UI or for parts of the UI that will display user-generated content, such as a blog post that supports Markdown or other markup. Using element selectors brazenly can lead to bad practices, such as over-qualifying selectors or having to overwrite too many declarations in your subsequent rules, so it’s better to keep its use in check before it becomes a problem.

## File Structure

This is what a barebones directory set up for SEAMS would look like…

```
stylesheets/
  augmentations/
  elements/
  modules/
  skins/
  structures/
```

Say we wanted to add a footer Structure to our app. This is what our `stylesheets` directory would look like now…

```
stylesheets/
  augmentations/
  elements/
  modules/
  skins/
  structures/
    footer/
      footer.scss
      footer-skin.scss
```

When creating Modules and Structures, you create a new directory and file named after it. You’ll likely need to create a Skin file also so the parts that need to be rebranded can be done so without having to load unnecessary rules.

## An Example

Let’s look at how you would organize a ubiquitous header and the parts it contains.

Since we’re placing this part of UI in a consistent location on every page and it’s likely to have other parts inside it, it makes sense to create it as a Structure.

Our header will have three different parts in it: a logo, a navigation list and a search bar. The logo is simple enough – at most composed of two HTML elements – and we might reuse it in many Modules and Structures, so we should create it as an Element. Our search bar will likely be reused as well (see the pattern?) but since it relies on containing multiple Elements to provide its functionality we should create it as Modules.

The navigation list could be created as either a Module or an Element, it depends on how complex you’re planning to make it. If you’re just displaying a list of links, all consistent in appearance other than a few Augmentations used to alter specific ones, then create is as an Element. But if you see the list having to contain multiple elements or have an appearance that’s very specific to the Structure it’s inside of, create it as a Module.

Here’s how we would organize our header’s structure in CSS and what files we’d create. Let’s assume our files are places at the root of the folder we use to keep CSS files, we’d store our header rules in `structures/header`

### header.scss
```
.header
  .nav
  .nav-item
  .nav-link
  .logo
  .logo-link
```

### header-aug.scss
```
.header
  &.is-reversed
  .nav-link.has-icon
```

### header-skin.scss
```
.header
  // declarations that could be changed by a theme go here as well
  &.is-inverted
  &.is-transparent
```

You don’t need to separate your rules into this granular of a structure if you don’t want to and it’s common to keep your Augmentations in the same file you’d write the Module in. But it is recommended that you have a separate file for your Skins if your project deals with themes, to make the compilation efficient when you make changes.

## Conclusion

Feel free to bend or break the rules I’ve laid out in this document. I want SEAMS to be a living methodology that improves as more people work with the system and find ways to improve it.

![](https://draftin.com:443/images/8084?token=RYifUelFT7lBGvYXg7HjL5fUVmJ5Eg6HBpVVbWoix73VHPQwCQUPPwM-yf8c7J9xStna9zFlAbkhS-lHWHSSUIg) 
