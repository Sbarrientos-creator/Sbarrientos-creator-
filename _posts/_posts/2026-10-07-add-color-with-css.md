---
layout: post
title: "Add Color and Style with CSS"
categories: code
author: CodeCapital Team
---

HTML builds the structure of a page. **CSS** (Cascading Style Sheets) controls how it looks:
colors, fonts, spacing and layout.

## Step 1: Start with your HTML page

Use the page from our [first HTML tutorial]({{ '/2026/10/05/your-first-html-page.html' | relative_url }}),
and add a `<style>` section inside the `<head>`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Styled Page</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        background-color: #f4f8ff;
        margin: 40px;
      }
      h1 {
        color: #2a7ae2;
      }
      p {
        font-size: 18px;
        line-height: 1.5;
      }
      a {
        color: darkgreen;
      }
    </style>
  </head>
  <body>
    <h1>Hello, world!</h1>
    <p>Now my page has color and style.</p>
    <a href="https://www.wikipedia.org">Visit Wikipedia</a>
  </body>
</html>
```

## How CSS rules work

Every rule has three parts:

```css
selector {
  property: value;
}
```

- **Selector:** which element to style (`h1`, `p`, `body`).
- **Property:** what you want to change (`color`, `font-size`).
- **Value:** how you want to change it (`blue`, `18px`).

## Common properties

| Property | What it does | Example |
|---|---|---|
| `color` | Text color | `color: red;` |
| `background-color` | Background color | `background-color: #eef5ff;` |
| `font-size` | Text size | `font-size: 20px;` |
| `margin` | Space outside an element | `margin: 20px;` |
| `padding` | Space inside an element | `padding: 10px;` |
| `border` | A line around an element | `border: 2px solid black;` |

## Challenge

1. Change the heading color to your favorite color.
2. Add a `border` and `padding` to the paragraph.
3. Create a second paragraph with a different color.

**Tip:** colors can be names (`blue`), hex codes (`#2a7ae2`) or RGB values.
