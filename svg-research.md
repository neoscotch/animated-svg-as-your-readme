# GitHub specific notes on SVG `<foreignObject>`

## original credit for the trick: https://github.com/Richienb/Richienb in this [pr](https://github.com/sindresorhus/sindresorhus/pull/9) 

### I took everything on this page from [mozilla](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/foreignObject) and edited it for this specific use case


The  [`<foreignObject>`](https://raw.githubusercontent.com/neoscotch/animated-svg-as-your-readme/c82536ac7e650bcd779755031d560ea59543888f/example.svg) element includes elements from a different XML namespace. 

In the context of a browser, it is most likely (X)HTML.

This example works properly when viewed directly

https://raw.githubusercontent.com/neoscotch/animated-svg-as-your-readme/c82536ac7e650bcd779755031d560ea59543888f/example.svg

but, when you use it on your GH readme it shows as a picture, but doesnt function

```xml
<svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
  <style> div {
      color: white;
      font: 18px serif;
      height: 100%;
      overflow: auto;
    } </style>

  <polygon points="5,5 195,10 185,185 10,195" />

  <!-- Common use case: embed HTML text into SVG -->
  <foreignObject x="20" y="20" width="160" height="160">
    <!--
      In the context of SVG embedded in an HTML document, the XHTML
      namespace could be omitted, but it is mandatory in the
      context of an SVG document
    -->
    <div xmlns="http://www.w3.org/1999/xhtml">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      Sed mollis mollis mi ut ultricies. Nullam magna ipsum,
      porta vel dui convallis, rutrum imperdiet eros. Aliquam
      erat volutpat.
    </div>
  </foreignObject>
</svg>
```
This image should scroll but on GH it doesnt work.
GH also makes the entire svg a link

<img src="https://github.com/neoscotch/animated-svg-as-your-readme/blob/main/example.svg" height="130px"></img>
  
  
[Attributes](#attributes "Permalink to Attributes")
---------------------------------------------------

[height](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/height)

The height of the foreignObject. _Value type_: [length](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#length) | [percentage](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#percentage) ; _Default value_: `auto`; _Animatable_: **yes**

[width](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/width)

The width of the foreignObject. _Value type_: [length](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#length)|[percentage](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#percentage) ; _Default value_: `auto`; _Animatable_: **yes**

[x](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/x)

The x coordinate of the foreignObject. _Value type_: [length](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#length)|[percentage](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#percentage) ; _Default value_: `0`; _Animatable_: **yes**

[y](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/y)

The y coordinate of the foreignObject. _Value type_: [length](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#length)|[percentage](https://developer.mozilla.org/en-US/docs/Web/SVG/Content_type#percentage) ; _Default value_: `0`; _Animatable_: **yes**

>## Starting with SVG2, `x`, `y`, `width`, and `height` are _Geometry Properties_, meaning those attributes can also be used as CSS properties for that element.

### [Global attributes](#global_attributes "Permalink to Global attributes")

[Core Attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Core)

Most notably: `[id](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/id)`, `[tabindex](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/tabindex)`

[Styling Attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Styling)

`[class](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/class)`, `[style](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/style)`

[Conditional Processing Attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Conditional_Processing)

Most notably: `[requiredExtensions](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/requiredExtensions "This is a link to an unwritten page")`, `[systemLanguage](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/systemLanguage)`

Event Attributes

[Global event attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Events#global_event_attributes), [Graphical event attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Events#graphical_event_attributes), [Document event attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Events#document_event_attributes), [Document element event attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Events#document_element_event_attributes)

[Presentation Attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Presentation)

Most notably: `[clip-path](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/clip-path)`, `[clip-rule](/en-US/docs/Web/SVG/Attribute/clip-rule)`, `[color](/en-US/docs/Web/SVG/Attribute/color)`, `[color-interpolation](/en-US/docs/Web/SVG/Attribute/color-interpolation)`, `[color-rendering](/en-US/docs/Web/SVG/Attribute/color-rendering)`, `[cursor](/en-US/docs/Web/SVG/Attribute/cursor)`, `[display](/en-US/docs/Web/SVG/Attribute/display)`, `[fill](/en-US/docs/Web/SVG/Attribute/fill)`, `[fill-opacity](/en-US/docs/Web/SVG/Attribute/fill-opacity)`, `[fill-rule](/en-US/docs/Web/SVG/Attribute/fill-rule)`, `[filter](/en-US/docs/Web/SVG/Attribute/filter)`, `[mask](/en-US/docs/Web/SVG/Attribute/mask)`, `[opacity](/en-US/docs/Web/SVG/Attribute/opacity)`, `[pointer-events](/en-US/docs/Web/SVG/Attribute/pointer-events)`, `[shape-rendering](/en-US/docs/Web/SVG/Attribute/shape-rendering)`, `[stroke](/en-US/docs/Web/SVG/Attribute/stroke)`, `[stroke-dasharray](/en-US/docs/Web/SVG/Attribute/stroke-dasharray)`, `[stroke-dashoffset](/en-US/docs/Web/SVG/Attribute/stroke-dashoffset)`, `[stroke-linecap](/en-US/docs/Web/SVG/Attribute/stroke-linecap)`, `[stroke-linejoin](/en-US/docs/Web/SVG/Attribute/stroke-linejoin)`, `[stroke-miterlimit](/en-US/docs/Web/SVG/Attribute/stroke-miterlimit)`, `[stroke-opacity](/en-US/docs/Web/SVG/Attribute/stroke-opacity)`, `[stroke-width](/en-US/docs/Web/SVG/Attribute/stroke-width)`, `[transform](/en-US/docs/Web/SVG/Attribute/transform)`, `[vector-effect](/en-US/docs/Web/SVG/Attribute/vector-effect)`, `[visibility](/en-US/docs/Web/SVG/Attribute/visibility)`

Aria Attributes

`aria-activedescendant`, `aria-atomic`, `aria-autocomplete`, `aria-busy`, `aria-checked`, `aria-colcount`, `aria-colindex`, `aria-colspan`, `aria-controls`, `aria-current`, `aria-describedby`, `aria-details`, `aria-disabled`, `aria-dropeffect`, `aria-errormessage`, `aria-expanded`, `aria-flowto`, `aria-grabbed`, `aria-haspopup`, `aria-hidden`, `aria-invalid`, `aria-keyshortcuts`, `aria-label`, `aria-labelledby`, `aria-level`, `aria-live`, `aria-modal`, `aria-multiline`, `aria-multiselectable`, `aria-orientation`, `aria-owns`, `aria-placeholder`, `aria-posinset`, `aria-pressed`, `aria-readonly`, `aria-relevant`, `aria-required`, `aria-roledescription`, `aria-rowcount`, `aria-rowindex`, `aria-rowspan`, `aria-selected`, `aria-setsize`, `aria-sort`, `aria-valuemax`, `aria-valuemin`, `aria-valuenow`, `aria-valuetext`, `role`
