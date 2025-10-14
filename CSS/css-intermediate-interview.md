# 🎨 Intermediate CSS Interview Questions & Answers

A collection of CSS questions designed to test practical and conceptual understanding at an intermediate level.

---

## 1. What is the difference between `relative`, `absolute`, and `fixed` positioning?
**Answer:**
- **relative** → positions element **relative to its normal position**  
- **absolute** → positions element **relative to the first positioned ancestor**  
- **fixed** → positions element **relative to the viewport** (stays fixed on scroll)

```css
div.relative { position: relative; top: 10px; }
div.absolute { position: absolute; top: 0; left: 0; }
div.fixed { position: fixed; bottom: 0; right: 0; }
```

---

## 2. What is the difference between `em`, `rem`, and `px`?
**Answer:**
- `px`: absolute unit  
- `em`: relative to **parent element’s font size**  
- `rem`: relative to **root element’s font size**

```css
p { font-size: 1.2em; }
h1 { font-size: 2rem; }
```

---

## 3. What are pseudo-classes and pseudo-elements?
**Answer:**
- **Pseudo-classes**: style elements in a specific **state** (`:hover`, `:focus`)
- **Pseudo-elements**: style **parts of elements** (`::before`, `::after`)

```css
button:hover { background: blue; }
p::first-letter { font-size: 2em; color: red; }
```

---

## 4. Difference between `display: none` and `visibility: hidden`?
**Answer:**
- `display: none` → removes element from the layout.  
- `visibility: hidden` → hides element but keeps its space reserved.

---

## 5. What is the difference between `inline`, `block`, and `inline-block`?
| Property | Description |
|-----------|--------------|
| inline | Doesn’t start on new line, can’t set width/height |
| block | Starts on new line, takes full width |
| inline-block | Inline but respects width and height |

---

## 6. Explain the difference between `position: sticky` and `fixed`.
**Answer:**
- `sticky`: behaves like relative until a threshold, then fixed.  
- `fixed`: always fixed relative to viewport.

```css
header { position: sticky; top: 0; }
```

---

## 7. What are CSS variables?
Custom properties for reusable values.

```css
:root { --main-color: #3498db; }
button { background: var(--main-color); }
```

---

## 8. What is the difference between `min-width`, `max-width`, and `width`?
- `width`: sets exact width  
- `min-width`: minimum size  
- `max-width`: maximum size  

---

## 9. What is the difference between `overflow: hidden`, `scroll`, and `auto`?
| Value | Behavior |
|--------|-----------|
| hidden | hides overflow |
| scroll | always shows scrollbar |
| auto | shows scrollbar if needed |

---

## 10. Explain CSS specificity.
**Answer:**
Determines which CSS rule takes precedence.

| Selector | Weight |
|-----------|---------|
| Inline | 1000 |
| ID | 100 |
| Class | 10 |
| Element | 1 |

---

## 11. What are media queries?
Used to make responsive layouts.

```css
@media (max-width: 600px) {
  body { font-size: 14px; }
}
```

---

## 12. What are CSS transitions and animations?
- **Transition**: smooth change on event (e.g., hover)  
- **Animation**: uses keyframes for continuous animation

```css
button { transition: background 0.3s; }
button:hover { background: red; }
```

---

## 13. What is `z-index`?
Controls stacking order. Works only with positioned elements.

---

## 14. Difference between Flexbox and Grid?
| Feature | Flexbox | Grid |
|----------|----------|------|
| Dimension | 1D (row/column) | 2D (rows + columns) |
| Use Case | Aligning items | Layout structure |

---

## 15. How to center a div both vertically and horizontally?
**Using Flexbox**
```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
```

**Using Grid**
```css
.container {
  display: grid;
  place-items: center;
  height: 100vh;
}
```

---

## 16. What is the difference between `float` and `flex`?
- `float` was for text wrapping (old layouts).  
- `flex` is for modern responsive layouts.

---

## 17. What are `:nth-child()` and `:nth-of-type()` used for?
Select elements based on order.

```css
li:nth-child(odd) { background: #eee; }
li:nth-of-type(2) { color: red; }
```

---

## 18. What are `clip-path` and `mask` used for?
To shape or hide portions of an element.

```css
img { clip-path: circle(50% at 50% 50%); }
```

---

## 19. How does `box-sizing` work?
Determines how total width/height are calculated.

```css
* { box-sizing: border-box; }
```

| Value | Description |
|--------|--------------|
| content-box | Default. Width excludes padding & border. |
| border-box | Width includes padding & border. |

---

## 20. What are CSS preprocessors (SASS/LESS)?
They add **variables**, **nesting**, and **mixins** to CSS, compiled into plain CSS.

---

