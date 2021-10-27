[Jump to Table of Contents](#toc)

Contents
--------

1.  1.  [12.1. Overview](https://www.w3.org/TR/SVG2/embedded.html#Overview)
    2.  [12.2. Placement of the embedded content](https://www.w3.org/TR/SVG2/embedded.html#Placement)
    3.  [12.3. The ‘image’ element](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)
    4.  [12.4. HTML elements in SVG subtrees](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)
    5.  [12.5. The ‘foreignObject’ element](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)
    6.  [12.6. DOM interfaces](https://www.w3.org/TR/SVG2/embedded.html#DOMInterfaces)
        1.  [12.6.1. Interface SVGImageElement](https://www.w3.org/TR/SVG2/embedded.html#InterfaceSVGImageElement)
        2.  [12.6.2. Interface SVGForeignObjectElement](https://www.w3.org/TR/SVG2/embedded.html#InterfaceSVGForeignObjectElement)

12.1. Overview[](#Overview)
---------------------------

Embedded content is content that imports another resource into the document, or content from another vocabulary that is inserted into the document. This is the same definition as [HTML's](https://html.spec.whatwg.org/multipage/) [embedded content](https://html.spec.whatwg.org/multipage/embedded-content.html#embedded-content).

SVG supports embedded content with the use of ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ and ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ elements.

Additionally SVG allows embedded content using HTML ['video'](https://html.spec.whatwg.org/multipage/media.html#the-video-element), ['audio'](https://html.spec.whatwg.org/multipage/media.html#the-audio-element), ['iframe'](https://html.spec.whatwg.org/multipage/iframe-embed-object.html#the-iframe-element) and ['canvas'](https://html.spec.whatwg.org/multipage/canvas.html#the-canvas-element) elements.

Except ‘[canvas](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ and ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’, embedded content is compatible with [Resource Hints](https://www.w3.org/TR/resource-hints/) for prioritizing downloading of external resources.

12.2. Placement of the embedded content[](#Placement)
-----------------------------------------------------

The [x](https://www.w3.org/TR/SVG2/geometry.html#XProperty), [y](https://www.w3.org/TR/SVG2/geometry.html#YProperty), [width](https://www.w3.org/TR/SVG2/geometry.html#Sizing), and [height](https://www.w3.org/TR/SVG2/geometry.html#Sizing) geometry properties specify the rectangular region into which the embedded content is positioned (the positioning rectangle). The [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle) is used as the bounding box of the element; note, however, that graphics may overflow the positioning rectangle, depending on the value of the [overflow](https://www.w3.org/TR/SVG2/render.html#OverflowAndClipProperties) property.

The elements in the HTML namespace do not have SVG presentation attributes for the geometry properties. Most of these elements, however, accept the HTML `width` and `height` [dimensional attributes](https://html.spec.whatwg.org/multipage/embedded-content-other.html#dimension-attributes), which are used as presentational hints to set default values for the corresponding sizing properties.

The HTML dimensional attributes must be parsed and interpretted as defined in the HTML specification \[[HTML](https://www.w3.org/TR/SVG2/refs.html#ref-html)\]. Specifically, they only accept integer values, not CSS lengths with units. On a ‘[canvas](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ element, [the attributes](https://html.spec.whatwg.org/multipage/canvas.html#the-canvas-element:the-canvas-element-19) are slightly different: they affect the rendered bitmap, not only its layout.

The [x](https://www.w3.org/TR/SVG2/geometry.html#XProperty) and [y](https://www.w3.org/TR/SVG2/geometry.html#YProperty) geometry properties can only be set on HTML-namespaced elements via CSS.

When the embedded content consists of a single referenced resource (e.g., an ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’, ‘[video](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’, or ‘[canvas](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’), the dimensions of the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle), in the current coordinate system after applying all transforms, define the [specified size](https://www.w3.org/TR/css3-images#specified-size) for the embedded object. A [concrete object size](https://www.w3.org/TR/css3-images#concrete-object-size) and final position must be determined for the object using the [Default Sizing Algorithm](https://www.w3.org/TR/css3-images/#default-sizing) defined for replaced elements in CSS layout \[[css-images-3](https://www.w3.org/TR/SVG2/refs.html#ref-css-images-3)\]. The [object-fit](https://www.w3.org/TR/css3-images/#the-object-fit) and [object-position](https://www.w3.org/TR/css3-images/#the-object-position) affect the final position and size of the object, and may cause it to be extend beyond the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle). In that case, the [overflow](https://www.w3.org/TR/SVG2/render.html#OverflowAndClipProperties) property determines whether the rendered object should be clipped to its [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle).

When the embedded content consists of a document fragment (e.g., a ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’, an ‘[audio](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ or ‘[video](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ with user-agent generated controls, or a ‘[video](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’, ‘[audio](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’, or ‘[canvas](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ displaying HTML fallback content), the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle) defines the bounds of a [containing block](https://www.w3.org/TR/CSS21/visuren.html#containing-block) for laying out the child content using CSS. The scale of the containing block is defined in the current coordinate system, including all explicit and implicit (e.g., ‘[viewBox](https://www.w3.org/TR/SVG2/coords.html#ViewBoxAttribute)’) transformations. The ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’, or other element that is positioned using SVG layout attributes, is implicitly [absolutely-positioned](https://www.w3.org/TR/CSS21/visuren.html#propdef-position) for the purposes of CSS layout. As a result, any absolutely-positioned child elements are positioned relative to this containing block. Again, the [overflow](https://www.w3.org/TR/SVG2/render.html#OverflowAndClipProperties) property determines whether content that extends outside the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle) will be hidden.

A value of zero for either [width](https://www.w3.org/TR/SVG2/geometry.html#Sizing) or [height](https://www.w3.org/TR/SVG2/geometry.html#Sizing) disables rendering of the element and its embedded content.

The 'auto' value for [width](https://www.w3.org/TR/SVG2/geometry.html#Sizing) or [height](https://www.w3.org/TR/SVG2/geometry.html#Sizing) is used to size the corresponding element automatically based on the [intrinsic dimensions](https://www.w3.org/TR/css3-images#intrinsic-dimensions) or [intrinsic aspect ratio](https://www.w3.org/TR/css3-images#intrinsic-aspect-ratio) of the referenced resource. Computation of automatically-sized values follows the [Default Sizing Algorithm](https://www.w3.org/TR/css3-images/#default-sizing) defined for replaced elements in CSS layout \[[css-images-3](https://www.w3.org/TR/SVG2/refs.html#ref-css-images-3)\]. In particular, when the referenced resource does not have an intrinsic size (such as an ‘[iframe](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ or an image types with no defined dimensions), it is assumed to have a width of 300px and a height of 150px.

CSS positioning properties (e.g. top and margin) have no effect when positioning the embedded content element in the SVG coordinate system. They can, however, be used to position child elements of a ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ or HTML embedding element.

12.3. The ‘image’ element[](#ImageElement)
------------------------------------------

The ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element indicates that the contents of a complete file are to be rendered into a given rectangle within the current user coordinate system. The ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element can refer to raster image files such as PNG or JPEG or to files with MIME type of "image/svg+xml". [Conforming SVG viewers](https://www.w3.org/TR/SVG2/conform.html#ConformingSVGViewers) need to support at least PNG, JPEG and SVG format files. SVG files must be processed in [secure animated mode](https://www.w3.org/TR/SVG2/conform.html#secure-animated-mode) if the current document supports animation, or in [secure static mode](https://www.w3.org/TR/SVG2/conform.html#secure-static-mode) if the current document is static.

The result of processing an ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ is always a four-channel RGBA result. When an ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element references an image (such as many PNG or JPEG files) which only has three channels (RGB), then the effect is as if the object were converted into a 4-channel RGBA image with the alpha channel uniformly set to 1. For a single-channel (grayscale) raster image, the effect is as if the object were converted into a 4-channel RGBA image, where the single channel from the referenced object is used to compute the three color channels and the alpha channel is uniformly set to 1.

The ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute determines how the referenced image is scaled and positioned to fit into the [concrete object size](https://www.w3.org/TR/css3-images#concrete-object-size) determined from the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle) and the [object-fit](https://www.w3.org/TR/css3-images/#the-object-fit) and [object-position](https://www.w3.org/TR/css3-images/#the-object-position) properties. The result of applying this attribute defines an image-rendering rectangle used for actual image rendering. When the referenced image is an SVG, the [image-rendering rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermImageRenderingRectangle) defines the [SVG viewport](https://www.w3.org/TR/SVG2/coords.html#TermSVGViewport) used for rendering that SVG.

The ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ calculations are applied _after_ determining the [concrete object size](https://www.w3.org/TR/css3-images#concrete-object-size), and only have an effect if that size does not match the [intrinsic aspect ratio](https://www.w3.org/TR/css3-images#intrinsic-aspect-ratio) of the embedded image. If a value of [object-fit](https://www.w3.org/TR/css3-images/#the-object-fit) is used that ensures that the concrete object size matches the intrinsic aspect ratio (i.e., any value other than the default fill), then the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ value will have no effect; the [image-rendering rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermImageRenderingRectangle) will be that determined when scaling and positioning the object with CSS. The ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute can therefore be safely used as a fallback for most values of [object-fit](https://www.w3.org/TR/css3-images/#the-object-fit) and [object-position](https://www.w3.org/TR/css3-images/#the-object-position); it must be explicitly set to none to turn off aspect ratio control, regardless of [object-fit](https://www.w3.org/TR/css3-images/#the-object-fit) value.

The aspect ratio to use when evaluating the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute is defined by the [intrinsic aspect ratio](https://www.w3.org/TR/css3-images#intrinsic-aspect-ratio) of the referenced content. For an SVG file, the aspect ratio is defined in [Intrinsic sizing properties of SVG content"](https://www.w3.org/TR/SVG2/coords.html#SizingSVGInCSS). For most raster content (PNG, JPEG) the pixel width and height of the image file define an intrinsic aspect ratio. Where the embedded image does not have an [intrinsic aspect ratio](https://www.w3.org/TR/css3-images#intrinsic-aspect-ratio) (e.g. an SVG file with neither ‘[viewBox](https://www.w3.org/TR/SVG2/coords.html#ViewBoxAttribute)’ attribute nor explicit dimensions for the [outermost svg element](https://www.w3.org/TR/SVG2/struct.html#TermOutermostSVGElement)) the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute is ignored; the embedded image is drawn to fill the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle) defined by the geometry properties on the ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element.

For example, if the image element referenced a PNG or JPEG and preserveAspectRatio="xMinYMin meet", then the aspect ratio of the raster would be preserved (which means that the scale factor from image's coordinates to current user space coordinates would be the same for both X and Y), the raster would be sized as large as possible while ensuring that the entire raster fits within the viewport, and the top/left of the raster would be aligned with the top/left of the viewport as defined by the attributes [x](https://www.w3.org/TR/SVG2/geometry.html#XProperty), [y](https://www.w3.org/TR/SVG2/geometry.html#YProperty), [width](https://www.w3.org/TR/SVG2/geometry.html#Sizing) and [height](https://www.w3.org/TR/SVG2/geometry.html#Sizing) on the ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element.  If the value of ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ was 'none' then aspect ratio of the image would not be preserved. The image would be fit such that the top/left corner of the raster exactly aligns with coordinate ([x](https://www.w3.org/TR/SVG2/geometry.html#XProperty), [y](https://www.w3.org/TR/SVG2/geometry.html#YProperty)) and the bottom/right corner of the raster exactly aligns with coordinate ([x](https://www.w3.org/TR/SVG2/geometry.html#XProperty)+[width](https://www.w3.org/TR/SVG2/geometry.html#Sizing), [y](https://www.w3.org/TR/SVG2/geometry.html#YProperty)+[height](https://www.w3.org/TR/SVG2/geometry.html#Sizing)).

For ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ elements embedding an SVG image, the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute on the root element in the referenced SVG image must be ignored, and instead treated as if it had a value of none. (see ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ for details). This ensures that the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute on the referencing ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ has its intended effect, even if it is none.

When the value of the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute on the ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ is _not_ none, the [image-rendering rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermImageRenderingRectangle) determined from the properties of the ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element will exactly match the embedded SVG's intrinsic aspect ratio. Ignoring the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ attribute from the embedded SVG will therefore not usually have any effect. The exception is if the aspect ratio of that image is determined from absolute values for the [width](https://www.w3.org/TR/SVG2/geometry.html#Sizing) and [height](https://www.w3.org/TR/SVG2/geometry.html#Sizing) attributes which _do not_ match its ‘[viewBox](https://www.w3.org/TR/SVG2/coords.html#ViewBoxAttribute)’ aspect ratio. This is an unusual situation that authors are advised to avoid, for many reasons.

The user agent stylesheet sets the value of the [overflow](https://www.w3.org/TR/SVG2/render.html#OverflowAndClipProperties) property on ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element to hidden. Unless over-ridden by the author, images will therefore be clipped to the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle) defined by the geometry properties.

For ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ elements embedding an SVG image, two different [overflow](https://www.w3.org/TR/SVG2/render.html#OverflowAndClipProperties) values apply. The value specified on the ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element determines whether the [image-rendering rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermImageRenderingRectangle) is clipped to the [positioning rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermPositioningRectangle). The value on the root element of the referenced SVG determines whether the graphics are clipped to the [image-rendering rectangle](https://www.w3.org/TR/SVG2/embedded.html#TermImageRenderingRectangle).

New in SVG 2. Previous versions of SVG required that the [overflow](https://www.w3.org/TR/SVG2/render.html#OverflowAndClipProperties) (and also [clip](https://drafts.fxtf.org/css-masking-1/#propdef-clip)) property on the embedded SVG be ignored. The new rules ensure that an overflowing slice layout can be safely used without compromising the overflow control from the referenced image.

To link into particular view of an embedded SVG image, authors can use media fragments as defined in [Linking into SVG content](https://www.w3.org/TR/SVG2/linking.html#LinksIntoSVG). To crop to a specific section of a raster image, authors can use _Basic media fragments identifiers_ \[[Media Fragments URI 1.0 (basic)](https://www.w3.org/TR/SVG2/refs.html#ref-media-frags)\]. Either type of fragment may affect the [intrinsic dimensions](https://www.w3.org/TR/css3-images#intrinsic-dimensions) and/or [intrinsic aspect ratio](https://www.w3.org/TR/css3-images#intrinsic-aspect-ratio) of the image.

The resource referenced by the ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element represents a separate document which generates its own parse tree and document object model (if the resource is XML). Thus, there is no inheritance of properties into the image.

Unlike ‘[use](https://www.w3.org/TR/SVG2/struct.html#UseElement)’, the ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element cannot reference elements within an SVG file.

_Attribute definitions:_

| Name | Value | Initial value | Animatable |
| --- | --- | --- | --- |
| crossorigin | \[ anonymous | use-credentials \]? | (see HTML definition of attribute) | yes |

The crossorigin attribute is a [CORS settings attribute](https://html.spec.whatwg.org/multipage/urls-and-fetching.html#cors-settings-attribute), and unless otherwise specified follows the same processing rules as in HTML \[[HTML](https://www.w3.org/TR/SVG2/refs.html#ref-html)\].

| Name | Value | Initial value | Animatable |
| --- | --- | --- | --- |
| href | URL [\[URL\]](https://www.w3.org/TR/SVG2/types.html#attribute-url) | (none) | yes |

An [URL reference](https://www.w3.org/TR/SVG2/linking.html#URLReference) identifying the image to render. Refer to the common handling defined for [URL reference attributes](https://www.w3.org/TR/SVG2/linking.html#linkRefAttrs) and [deprecated XLink attributes](https://www.w3.org/TR/SVG2/linking.html#XLinkRefAttrs).

The URL is processed and the resource is fetched [as described in the Linking chapter](https://www.w3.org/TR/SVG2/linking.html#processingURL).

<?xml version="1.0" standalone="no"?>
<svg width="4in" height="3in"
     xmlns="http://www.w3.org/2000/svg">
  <desc>This graphic links to an external image
  </desc>
  <image x="200" y="200" width="100px" height="100px"
         href="myimage.png">
    <title>My image</title>
  </image>
</svg>

Since image references always refer to a complete document, a target-only URL is treated as a link to the same file, which is rendered again as an independent embedded image. Since the embedded image is processed in a secure mode, its own embedded references are not processed, preventing infinite recursion.

```
<?xml version="1.0" standalone="no"?>
<svg width="5cm" height="3cm" viewBox="0 0 50 30"
     xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink">
  <title>Recursive SVG</title>
  <desc>An SVG with two recursive image reference to itself.
    One reference uses the file name as a relative URL,
    the other uses a target fragment only.
    When viewed in a processing mode that supports external file references,
    the embedded images should be rendered;
    however, the embedded image must be processed in secure mode,
    so the recursion only happens once.
    The appearance should be three nested red circles in a bulls-eye pattern;
    the innermost circle has solid fill because of target styles.
  </desc>
  <style type="text/css">
    #target:target {
      fill: red;
    }
  </style>
  <circle id="target"
          stroke="red" stroke-width="5" fill="none"
          cx="50%" cy="50%" r="12" />
  <image xlink:href="recursive-image.svg"
         x="25%" y="25%" width="50%" height="50%" />
  <image xlink:href="#target"
         x="45%" y="45%" width="10%" height="10%" />
</svg>
```

![](https://www.w3.org/TR/SVG2/images/embedded/recursive-image.png)

Example recursive-image

[View this example as SVG (SVG-enabled browsers only)](https://www.w3.org/TR/SVG2/images/embedded/recursive-image.svg)

12.4. HTML elements in SVG subtrees[](#HTMLElements)
----------------------------------------------------

The following HTML elements render when included in an SVG subtree as a child of a [container element](https://www.w3.org/TR/SVG2/struct.html#TermContainerElement) and when using the [HTML namespace](https://html.spec.whatwg.org/multipage/infrastructure.html#xml):

*   ['video'](https://html.spec.whatwg.org/multipage/media.html#the-video-element)
*   ['audio'](https://html.spec.whatwg.org/multipage/media.html#the-audio-element)
*   ['iframe'](https://html.spec.whatwg.org/multipage/iframe-embed-object.html#the-iframe-element)
*   ['canvas'](https://html.spec.whatwg.org/multipage/canvas.html#the-canvas-element)

```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:html="http://www.w3.org/1999/xhtml">
  <html:video src="file.mp4" controls="controls">
  </html:video>
</svg>
```

HTML elements, in the HTML namespace, used as children of ['video'](https://html.spec.whatwg.org/multipage/media.html#the-video-element), ['audio'](https://html.spec.whatwg.org/multipage/media.html#the-audio-element), ['iframe'](https://html.spec.whatwg.org/multipage/iframe-embed-object.html#the-iframe-element) and ['canvas'](https://html.spec.whatwg.org/multipage/canvas.html#the-canvas-element) elements within an SVG document fragment behave as specified in HTML. This applies in particular to [fallback content](https://html.spec.whatwg.org/multipage/dom.html#fallback-content); if fallback content is rendered, the embedded element behaves like an SVG ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ element to contain the HTML content. This would occur, for example, for a ‘[video](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ element if the user agent does not support the specified video formats, or for a ‘[canvas](https://www.w3.org/TR/SVG2/embedded.html#HTMLElements)’ element if scripting is disabled.

```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:html="http://www.w3.org/1999/xhtml">
  <html:video src="http://example.org/dummyvideo" controls="controls">
    <html:p>The video format is not supported by this browser.</html:p>
  </html:video>
</svg>
```

The HTML specification is applicable also for the ['track'](https://html.spec.whatwg.org/multipage/media.html#the-track-element) and ['source'](https://html.spec.whatwg.org/multipage/embedded-content.html#the-source-element) elements.

```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:html="http://www.w3.org/1999/xhtml">
  <html:video src="file.mp4" controls="controls">
    <html:source src="file.webm" type='video/webm;codecs="vp8, vorbis"'/>
    <html:source src="file.mp4" type='video/mp4;codecs="avc1.42E01E, mp4a.40.2"'/>
  </html:video>
</svg>
```

Currently, within an SVG subtree, these tagnames are not recognized by the HTML parser to be HTML-namespaced elements, although this may change in the future. Therefore, in order to include these elements within SVG, one of the following must be used:

*   An XML serialization that recognizes namespace declarations (such as stand-alone SVG or XHTML).
*   Namespaced elements as created via the `createElementNS` DOM API method.
*   A ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ element to wrap the HTML-namespaced elements, which will then be correctly parsed.

Other HTML elements in an SVG subtree, other than those inside a ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ element, must be treated as [unknown elements](https://www.w3.org/TR/SVG2/struct.html#UnknownElement) for rendering purposes.

Many HTML elements will be treated as a parse error by the HTML parser, causing the SVG fragment to terminate.

12.5. The ‘foreignObject’ element[](#ForeignObjectElement)
----------------------------------------------------------

SVG is designed to be compatible with other XML languages for describing and rendering other types of content. The ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ element allows for inclusion of elements in a non-SVG namespace which is rendered within a region of the SVG graphic using other user agent processes. The included foreign graphical content is subject to SVG transformations, filters, clipping, masking and compositing. Examples include inserting a [MathML](https://www.w3.org/TR/2003/REC-MathML2-20031021/) expression into an SVG drawing \[[MathML3](https://www.w3.org/TR/SVG2/refs.html#ref-mathml3)\], or adding a block of complex CSS-formatted HTML text or form inputs.

The HTML parser treats elements inside the ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ equivalent to elements inside an HTML document fragment. Any `svg` or `math` element, and their descendents, will be parsed as being in the SVG or MathML namespace, respectively; all other tags will be parsed as being in the HTML namespace.

SVG-namespaced elements within a ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ will not be rendered, except in the situation where a properly defined SVG fragment, including a root ‘[svg](https://www.w3.org/TR/SVG2/struct.html#SVGElement)’ element is defined as a descendent of the ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’.

A ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ may be used in conjunction with the ‘[switch](https://www.w3.org/TR/SVG2/struct.html#SwitchElement)’ element and the ‘[requiredExtensions](https://www.w3.org/TR/SVG2/struct.html#RequiredExtensionsAttribute)’ attribute to provide proper checking for user agent support and provide an alternate rendering in case user agent support is not available.

This specification does not define how ‘[requiredExtensions](https://www.w3.org/TR/SVG2/struct.html#RequiredExtensionsAttribute)’ values should be mapped to support for different XML languages; a future specification may do so.

It is not required that SVG user agent support the ability to invoke other arbitrary user agents to handle embedded foreign object types; however, all conforming SVG user agents would need to support the ‘[switch](https://www.w3.org/TR/SVG2/struct.html#SwitchElement)’ element and must be able to render valid SVG elements when they appear as one of the alternatives within a ‘[switch](https://www.w3.org/TR/SVG2/struct.html#SwitchElement)’ element.

It is expected that commercial Web browsers will support the ability for SVG to embed CSS-formatted HTML and also MathML content, with the rendered content subject to transformations and compositing defined in the SVG fragment. At this time, such a capability is not a requirement.

<?xml version="1.0" standalone="yes"?>
<svg width="4in" height="3in"
 xmlns = 'http://www.w3.org/2000/svg'>
  <desc>This example uses the 'switch' element to provide a
        fallback graphical representation of an paragraph, if
        XMHTML is not supported.</desc>
  <!-- The 'switch' element will process the first child element
       whose testing attributes evaluate to true.-->
  <switch>
    <!-- Process the embedded XHTML if the requiredExtensions attribute
         evaluates to true (i.e., the user agent supports XHTML
         embedded within SVG). -->
    <foreignObject width="100" height="50"
                   requiredExtensions="http://example.com/SVGExtensions/EmbeddedXHTML">
      <!-- XHTML content goes here -->
      <body xmlns="http://www.w3.org/1999/xhtml">
        <p>Here is a paragraph that requires word wrap</p>
      </body>
    </foreignObject>
    <!-- Else, process the following alternate SVG.
         Note that there are no testing attributes on the 'text' element.
         If no testing attributes are provided, it is as if there
         were testing attributes and they evaluated to true.-->
    <text font-size="10" font-family="Verdana">
      <tspan x="10" y="10">Here is a paragraph that</tspan>
      <tspan x="10" y="20">requires word wrap.</tspan>
    </text>
  </switch>
</svg>

12.6. DOM interfaces[](#DOMInterfaces)
--------------------------------------

### 12.6.1. Interface SVGImageElement[](#InterfaceSVGImageElement)

An [SVGImageElement](https://www.w3.org/TR/SVG2/embedded.html#InterfaceSVGImageElement) object represents an ‘[image](https://www.w3.org/TR/SVG2/embedded.html#ImageElement)’ element in the DOM.

```
\[Exposed=Window\]
interface **SVGImageElement** : [SVGGraphicsElement](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGGraphicsElement) {
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [x](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGImageElement__x);
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [y](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGImageElement__y);
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [width](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGImageElement__width);
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [height](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGImageElement__height);
  \[SameObject\] readonly attribute [SVGAnimatedPreserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#InterfaceSVGAnimatedPreserveAspectRatio) [preserveAspectRatio](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGImageElement__preserveAspectRatio);
  attribute DOMString? [crossOrigin](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGImageElement__crossOrigin);
};

[SVGImageElement](https://www.w3.org/TR/SVG2/embedded.html#InterfaceSVGImageElement) includes [SVGURIReference](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGURIReference);
```

The **x**, **y**, **width** and **height** IDL attributes [reflect](https://www.w3.org/TR/SVG2/types.html#TermReflect) the computed values of the [x](https://www.w3.org/TR/SVG2/geometry.html#XProperty), [y](https://www.w3.org/TR/SVG2/geometry.html#YProperty), [width](https://www.w3.org/TR/SVG2/geometry.html#Sizing) and [height](https://www.w3.org/TR/SVG2/geometry.html#Sizing) properties and their corresponding presentation attributes, respectively.

The **preserveAspectRatio** IDL attribute [reflects](https://www.w3.org/TR/SVG2/types.html#TermReflect) the ‘[preserveAspectRatio](https://www.w3.org/TR/SVG2/coords.html#PreserveAspectRatioAttribute)’ content attribute.

The **crossOrigin** IDL attribute [reflects](https://www.w3.org/TR/SVG2/types.html#TermReflect) the ‘[crossorigin](https://www.w3.org/TR/SVG2/embedded.html#ImageElementCrossoriginAttribute)’ content attribute.

### 12.6.2. Interface SVGForeignObjectElement[](#InterfaceSVGForeignObjectElement)

An [SVGForeignObjectElement](https://www.w3.org/TR/SVG2/embedded.html#InterfaceSVGForeignObjectElement) object represents a ‘[foreignObject](https://www.w3.org/TR/SVG2/embedded.html#ForeignObjectElement)’ in the DOM.

```
\[Exposed=Window\]
interface **SVGForeignObjectElement** : [SVGGraphicsElement](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGGraphicsElement) {
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [x](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGForeignObjectElement__x);
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [y](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGForeignObjectElement__y);
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [width](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGForeignObjectElement__width);
  \[SameObject\] readonly attribute [SVGAnimatedLength](https://www.w3.org/TR/SVG2/types.html#InterfaceSVGAnimatedLength) [height](https://www.w3.org/TR/SVG2/embedded.html#__svg__SVGForeignObjectElement__height);
};
```

The **x**, **y**, **width** and **height** IDL attributes [reflect](https://www.w3.org/TR/SVG2/types.html#TermReflect) the computed values of the [x](https://www.w3.org/TR/SVG2/geometry.html#XProperty), [y](https://www.w3.org/TR/SVG2/geometry.html#YProperty), [width](https://www.w3.org/TR/SVG2/geometry.html#Sizing) and [height](https://www.w3.org/TR/SVG2/geometry.html#Sizing) properties and their corresponding presentation attributes, respectively.
