---
{"dg-publish":true,"dg-permalink":"css","permalink":"/css/","dgHomeLink":true,"dgPassFrontmatter":false}
---


# CSS Notes

As many other developers out there, my background is not Computer Science, I studied Customs Administration and Foreign Trade and decided to switch careers.
But also, as many others, I struggled with learning all what we need to learn in this constant changin career that we choose. That's why some time ago I built a small site that helped me to reinforce my CSS knowledge, I called it <a href="https://css-notes.netlify.app/">CSS Notes</a>, right now it contains the 2 topics I was struggling the most at that moment "Positioning" and "Display".

I am currently taking Josh W Comeau's <a href="https://css-for-js.dev/">CSS for JS Developers</a> course, and I am planning to take my CSS notes into there just to remember myself of all what I have learned and also a quick way to look for my notes whenever I forget any concept again.

# Module 0 - Fundamentals Recap

## Pseudo-classes

### focus

HTML comes with interactive elements like buttons, links, and form inputs. When we interact with one of these elements (either by clicking on it or tabbing to it), it becomes focused. It'll capture keyboard input, so we can type into a form field or press "Enter" to follow a link.

The `:focus` pseudo-class allows us to apply styles exclusively when an interactive element has focus.

## Pseudo-elements

Pseudo-elements are like pseudo-classes, but they don't target a specific state. Instead, they target "sub-elements" within an element.

For example, we can style the placeholder text in a form input with `::placeholder`:

```
input::placeholder {
    color: goldenrod;
}
```

> In terms of syntax, pseudo-elements use two colons instead of one (::), though some pseudo-elements also support single-colon syntax.
> This is why they're called pseudo-elements — these selectors target elements in the DOM that we haven't explicitly created with HTML tags.

### before and after

These pseudo-elements are added inside the element, right before and after the element's content.
There is no significant difference in terms of performance between these two examples. ::before and ::after are really just secret spans, nothing more. It's syntactic sugar.

> There are also some accessibility concerns with ::before and ::after. Some screen readers? will try to vocalize the content.
> Others will ignore them entirely. This inconsistency is problematic.

## Combinators

```
nav a {
    color: red;
    font-weight: bold;
}
```

The term “combinator” refers to a character that combines multiple selectors. In this case, the space character combines nav and a to create a descendant selector.

In CSS, we can differentiate between children and descendants. Think of a family tree: a child is only one level down from the parent. A descendant might be 1 level down (child), 2 levels down (grandchild), 3 levels down…

## Colors

### HSL

```
hsl(hue as degrees, saturation, lightness)
hsl(344deg, 50%, 50%)
```

The first number has the deg suffix since it's in degrees (from 0° to 360°), and the next two numbers are percentages (from 0% to 100%).

### Transparency

Certain color formats allow us to supply an additional value for the alpha channel.

```
background-color: hsl(340deg 100% 50% / 1);
```

The `/` character is becoming a more common pattern in modern CSS. It isn't about division, it's about separation. The slash allows us to create groups of values. The first group is about the color. The second group is about its opacity.

## Units

When it comes to most things, we'll use pixels in this course. The big exception is typography.

### Ems

The `em` unit is an interesting fellow. It's a relative unit, equal to the font size of the current element.

If a heading has a font-size of 24px, and we give it a bottom padding of 2em, we can expect that the element will have 48px of cushion underneath it (2 × 24px).

How often should you use ems? I don't often reach for them. It can be very surprising when a tweak to font-size affects the spacing of descendant elements.

### Rems

The `rem` unit is quite a lot like the `em` unit, with one crucial difference: it's always relative to the root element, the <html> tag.

It behaves consistently and predictably, like pixels, but it respects user preferences when it comes to increasing/decreasing default font sizes.

> We shouldn't actually set a px font size on the html tag. This will override a user's chosen default font size.

### Percentages

The percentage unit is often used with width/height, as a way to consume a portion of the available space.

### Josh's Recommendations

- For typography, I generally use rem, because it has important accessibility benefits.

- When it comes to properties that relate to the box model — padding, border, margin — I usually use pixels. It's more intuitive than rem, and there isn't a clear accessibility win.
- For width/height, it'll depend on whether I want the element to be a fixed size, or a relative size. I might want one div to always be 250px wide, while another one should be 50% of the available space.
- For color, as we saw in the last lesson, I prefer hsl.

## Typography

- The default value for font weight is 400, and the bold keyword maps to 700.
- The `<strong>` HTML tag is meant to convey that an element is critically important or urgent, like “Warning: Product may explode if shaken”.

### Spacing

We can tweak the spacing of our characters in two ways.

- We can tweak the horizontal gap between characters using the letter-spacing property.
- We can tweak the vertical distance between lines using the line-height property.

### Line height and accessibility

In order to make our apps and websites as accessible as possible, we want to choose a pretty generous value for line-height. This will help those experiencing low vision conditions, as well as those with cognitive difficulties like dyslexia.

> The minimum recommended value is 1.5.

# Module 1 - Rendering Logic I

Default styles are part of the `user-agent stylesheet`. Each browser includes their own stylesheet full of base styles like this.
`"built-in" styles` — these are the rules that each browser comes with out-of-the-box, defined in the user-agent stylesheet.

## The Box Model

The four aspects that make up the box model are:

1. Content
1. Padding
1. Border
1. Margin

### Box Sizing

When we set our .box to have `width: 100%`, we're saying that the box's content size should be equal to the available space, 500px. The padding and border is added on top.


> [!NOTE] box-sizing
> The `box-sizing` CSS property allows us to change the rules for size calculations. The default value (content-box) only takes the inner content into account, but it offers an alternative value: `border-box`.
> With `box-sizing: border-box`, things behave **much** more intuitively.

### A new default

Instead of having to remember to swap `box-sizing` on every layout element, we can set it as the default value for all elements with this handy code snippet:

```
_,
_::before,
::after {
    box-sizing: border-box;
}

```

### Padding
Inner Space
Many developers believe that pixels are bad for accessibility. This is true when it comes to _font size_, but I actually think pixels are the best unit to use for padding (and other box model properties like margin/border).

**Padding percentages**

Can we use percentages for padding, like this?

.box {

  padding-top: 25%;

}

We _can_, but the result is surprising and counter-intuitive; **percentages always refer to the width of the element**, even when setting top/bottom padding.


```
padding-block: 20px;
padding-inline: 10px;
padding-inline-start: 0px;
```
>[!NOTE] Logical properties
>`padding-block` refers to the top and bottom (in top-down left-to-right languages like English). `padding-inline` refers to the left and right. `padding-inline-start` refers to the left.


### Border
There are three styles specific to border:
-   Border width (eg. `3px`, `1em`)
-   Border style (eg. `solid`, `dotted`)
-   Border color (eg. `hotpink`, `black`)

#### Border vs. Outline

A common stumbling block for devs is the distinction between `outline` and `border`. In some respects, they're quite similar! They both add a visual edge to a given element.

The core difference is that _outline doesn't affect layout_. Outline is kinda more like box-shadow; it's a cosmetic effect draped over an element, without nudging it around, or changing its size.

Never apply the following CSS:
```
button {
  outline: none;
}
```

The problem is that this totally breaks navigation for keyboard users; that ring is required for them to know which element is currently focused!

### Margin
Margin increases the space _around_ an element, giving it some breathing room.
With padding and border, only positive numbers (including 0) are supported. With margin, however, we can drop into the negatives.

Negative margin can affect the position of _all siblings_. 

## Auto margins
Two caveats:

-   **This only works for horizontal margin.** Setting top/bottom margin to `auto` is equivalent to setting it to `0px`
-   **This only works on elements with an explicit width**. Block elements will naturally grow to fill the available horizontal space, so we need to give our element a `width` in order to center it.
-
### Stretched content
Create a wrapper
apply negative margings

>[!NOTE] Stretched content outside of main content
> - Create a wrapper
> - Apply negative margings to the desired component
