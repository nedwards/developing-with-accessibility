# Developing With Accessibility In Mind
A guide for creating accessible content and components for your website.

## Basics

### Page Language

If the primary language of the web page has not been identified, screen reading software in general will read out the content in the same language as the default setting for the screen reader. So if your screen reader has English set as the default language, it will read out web page content in English. 

```html
<!-- Set the screenreader language -->
<html lang="en">
```

### Skip Links + Landmark Roles

Allows vision impaired and users with motor disabilities to quickly bypass the main navigation and jump to sections of the page.  The skip links correspond to the id of the landmark roles.

#### Best Practices

Initially hide the skip links from view but then display the skip links when they have keyboard `:focus`.  That way there is a visual representation when a users initially tabs through the page.  Hidding the skip links form keyboard `:focus` can cause visual confusion for physically handicapped users, as they cannot locate where on the page the focus currently is. 

```html
<!-- Skip links -->
<a class="sr-only" href="#header">Skip to header</a>
<a class="sr-only" href="#nav">Skip to navigation</a>
<a class="sr-only" href="#main">Skip to main content</a>
<a class="sr-only" href="#aside">Skip to side menu</a>
<a class="sr-only" href="#footer">Skip to footer</a>

<!-- Landmark roles -->
<header id="header" role="banner">
<nav class="nav" role="navigation">
<main id="main" class="content" role="main">
<aside id="aside" role="complementary">
<footer id="footer" class="footer" role="contentinfo">
<article role="article">
<section role="region">
<form action="" role="search">
```

## Styling

### Focus

Anything that is tabable or a navigation component, needs a visual indicator of the focus. Most browsers have this built in, but resets sometimes remove this.  To sensure a consistent style across browsers for focus you can set it like below.

```css
*:focus {
  outline: none;
  shadow-box: 12 12 blue;
}
```

### Visibility

Class to display content only for screenreaders. Invisible to the user on the page but screenreaders will still be able to pick up on the content.  Useful for providing more detailed descriptions on the page that are beneficial to visually impaired users.

```css
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0 0 0 0);
    border: 0;
}
```

Possible usage...

```html
<a href="http://www.google.com">
  <i class="icon-google" aria-hidden="true"></a>
  <span class="sr-only">External link to Google's homepage<span>
</a>
```


## Forms

### Labels + Inputs

To create the connection between a label and input you must have a label with a `for=""` attribute that links to the `id=""` of the input field.

```html
<form>
  <div>
    <label for="name">* Name:</label>
    <input type="text" value="name" id="name" required/>
  </div>
  <div>
    <label for="phone">Phone:</label>
    <input type="text" value="phone" id="phone"/>
  </div>
</form>  
```

### Input Errors

`aria-invalid` If a form input is required and has not been entered apply this tag along with `aria-describedby` to link the input error to the appropriate message for the user. The alert message also contains a `role="alert"` tag to ensure the user is notified.

```html
<div class="form-row">
  <label for="email">Email</label>
  <input name="email" id="email" required aria-invalid="true" aria-describedby="emailErrorMessage">
  <div class="error" id="emailErrorMessage" aria-hidden="false" role="alert" aria-atomic="true">
    <p>Please enter your email!</p>
  </div>
</div>
```

## Aria Live Regions

[Mozilla - live regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)

`aria-live`
- `"polite"` Any updates made to this region are announced if the user is not currently doing anything.
- `"assertive"` Any updates made to this region are announced to the user immediately.

`aria-relevant`
- `"additions"` Insertion of nodes into the live region should be considered relevant.
- `"removals"` Deletion of nodes should be considered relevant.
- `"text"` Changes to the textual content of existing nodes should be considered relevant.
- `"all"` Equivalent to additions removals text.

```html
<ul id="region" aria-live="polite" aria-relevant="additions removals">
	<!-- anything added or removed from within here will be notified via the screenreader to the user -->
</ul>
```

## Aria Controls

`aria-controls` is used to associate a control with the regions that it controls. Regions are identified via their ID, and multiple regions can be associated with a control using a space, e.g. `aria-controls="regionOne regionTwo"`.

```html
<button aria-controls="region">Click to remove hidden attribute and show div</button>

<div id="region" role="region" aria-relevant="additions" aria-live="polite">
  <div hidden>I will be read out by the screenreader once you click the button</div>
</div>
```



