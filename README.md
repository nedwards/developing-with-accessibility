# Accessibility in development
Guide for creating accessible websites

## Basics

### Page Language

If the primary language of the web page has not been identified, screen reading software in general will read out the content in the same language as the default setting for the screen reader. So if your screen reader has English set as the default language, it will read out web page content in English. 

``` html
<html lang="en">
```

### Skip Links + Landmark Roles

Allows vision impaired and users with motor disabilities to quickly bypass the main navigation and jump to sections of the page.

```html
<!-- Skip links -->
<a class="visuallyhidden" href="#header">Skip to header</a>
<a class="visuallyhidden" href="#nav">Skip to navigation</a>
<a class="visuallyhidden" href="#main">Skip to main content</a>
<a class="visuallyhidden" href="#aside">Skip to side menu</a>
<a class="visuallyhidden" href="#footer">Skip to footer</a>

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
