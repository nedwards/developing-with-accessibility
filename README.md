# Developing With Accessibility In Mind
A guide for creating accessible content and components for your website.

## Basics

### Page Language

If the primary language of the web page has not been identified, screen reading software in general will read out the content in the same language as the default setting for the screen reader. So if your screen reader has English set as the default language, it will read out web page content in English. 

```html
<!-- Set the screenreader language -->
<html lang="en">
```

### Page Titles

Users with visual disabilities rely on page titles to identify websites when they have multiple tabs open.
Below is an example with the title split into three sections.

```
<title>Page name - Section name - Business name</title>
```

The title of each page in your site should:
- Identify the subject of the Web page
- Make sense when read out of context
- Be short

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

### Tabindex

Always structure your code appropriately as this is the best solution rather than forcing the index order. Applying a positive `tabindex=""` order is not advised.

#### Do
- Use `tabindex="0"` to include an element in the natural tab order of the content
- Use `tabindex="-1"` to exclude it from the natural tab order of the page

#### Don't
- Use `tabindex="1"` to set a specific tab order

### Images

Text alternative should contain a short description conveying the essential information presented by the image.

```html
<img src="beach-sunset.jpg" alt="Picture of a sunset on the beach">
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
  <fieldset>
    <label for="name">* Name:</label>
    <input type="text" value="name" id="name" required/>
  </fieldset>
  <fieldset>
    <label for="phone">Phone:</label>
    <input type="text" value="phone" id="phone"/>
  </fieldset>
</form>  
```

### Radio Buttons

Radio buttons are probably one of a few instances where you don't have to link your label to the radio button group.  By providing a `legend` tag this will automatically be associated to the group for you.  You will also need to hide the `legend` tag via the `.sr-only` class.

```html
<fieldset tabindex="0">
  <label>Label name</label>
  <legend class="sr-only">Label name</legend>
  <ul>
    <li>
      <input type="radio" name="radiogroup" value="Option 1" id="radio-1" tabindex="0">
      <label for="radio-1">Option 1</label>
    </li>
    <li>
      <input type="radio" name="radiogroup" value="Option 2" id="radio-2" tabindex="0">
      <label for="radio-2">Option 2</label>
    </li>
  </ul>  
</fieldset>
```

### Input Errors

`aria-invalid` is required if a form input is in error. By applying `aria-invalid` to the input along with `aria-describedby` creates a link to the input error. The error alert message also contains a `role="alert"` tag to ensure the user is notified when an input is in error via the screenreader.

```html
<fieldset>
  <label for="email">Email</label>
  <input name="email" id="email" required aria-invalid="true" aria-describedby="emailErrorMessage">
  <div class="error" id="emailErrorMessage" aria-hidden="false" role="alert" aria-atomic="true">
    <p>Please enter your email!</p>
  </div>
</fieldset>
```

## Aria Hidden

`aria-hidden="true"` is very useful when you don't wish to display certain content for screenreaders.

### Icons

If using an icon font like FontAwesome, then be sure to hide the icon from screenreaders. Otherwise the icon `content=''` code will be read out to the user which is completely meaningless. Also be sure to provide a meaningful link description if an icon is being used as a link or navigation item.

``` html
<i aria-hidden=“true”></i>
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

## Tools

[Color Contrast Analyzer - Chrome Extension](https://chrome.google.com/webstore/detail/color-contrast-analyzer/dagdlcijhfbmgkjokkjicnnfimlebcll?hl=en)
Very useful for checking if a page meets WCAG 2.0 color contrast requirements.  Once running the test, any text on the page that does not have an outline around it is considered non compliant.

[ChromeVox - Chrome Extension](https://chrome.google.com/webstore/detail/chromevox/kgejglhpjiefppelpmljglcjbhoiplfn?hl=en)
Screenreader extension for Chrome.

[Accessibility Developer Tools - Chrome Extension](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb?hl=en)
Useful in auditing your page for any accessibility issues which will then be displayed within the developer console area.



