## aframe-troika-text

[![Version](http://img.shields.io/npm/v/aframe-troika-text.svg?style=flat-square)](https://npmjs.org/package/aframe-troika-text)
[![License](http://img.shields.io/npm/l/aframe-troika-text.svg?style=flat-square)](https://npmjs.org/package/aframe-troika-text)

This package provides an [A-Frame](https://aframe.io) component and primitive for rendering three-dimensional text using [Troika's text renderer](https://github.com/protectwise/troika/tree/master/packages/troika-3d-text).

It has similar performance and quality to A-Frame's built-in SDF `text` component, but brings some additional advantages:

* It reads font files directly (ttf, otf, woff) and does not require using an external tool to pre-generate SDF textures with the glyphs you think you'll need.

* It supports ligatures so you can use fonts like [Material Icons](https://material.io/resources/icons/).

* Rather than using a fully custom shader, it patches the built-in Three.js material shaders as needed, so you don't lose all the nice standard shader features like lighting and fog.


### API

This package registers both a _component_ (`<a-entity troika-text="value:Hello" />`) and a _primitive_ (`<a-troika-text value="Hello"></a-troika-text>`). Use whichever is most convenient for you.

I've attempted to keep the API as close as possible to that of A-Frame's default [text component](https://aframe.io/docs/master/components/text.html) and [a-text primitive](https://aframe.io/docs/master/primitives/a-text.html), however some things don't quite map exactly.

| Property on component | Attribute on primitive | Description                                                                                                 | Default Value                   |
|-----------------------|------------------------|-------------------------------------------------------------------------------------------------------------|---------------------------------|
| align                 | align                  | Multi-line text alignment (left, center, right, justify).                                                   | left                            |
| anchor                | anchor                 | Horizontal positioning (left, center, right, align).                                                        | center                          |
| baseline              | baseline               | Vertical positioning (top, center, bottom).                                                                 | center                          |
| color                 | color                  | Text color. This is a shortcut for specifying a custom material.                                            | white                           |
| depthOffset           | depth-offset           | Depth buffer offset to help prevent z-fighting. Negative numbers are closer to camera, positives further.   | 0                               |
| font                  | font                   | URL to a font file - can be a .ttf, .otf, or .woff (no .woff2)                                              | Roboto loaded from Google Fonts |
| fontSize              | font-size              | Em-height at which to render the font, in world units                                                       | 0.2                             |
| letterSpacing         | letter-spacing         | Letter spacing in meters.                                                                                   | 0                               |
| lineHeight            | line-height            | Line height as a multiple of the fontSize.                                                                  | *derived from font metrics*     |
| maxWidth              | max-width              | Maximum width of the text block at which text will start wrapping, in meters.                               | Infinity (no wrapping)          |
| overflowWrap          | overflow-wrap          | Controls how text wraps: "normal" to break at whitespace characters, or "break-word" to break within words. | normal                          |
| **value**             | value                  | The actual content of the text. Line breaks and tabs are supported (literal chars in HTML, `\n` or `\t` in JS). | ''                              |
| whiteSpace            | white-space            | How whitespace should be handled (i.e., normal, nowrap).                                                    | normal (behaves like pre-wrap)  |

Note: It does not currently follow how the built-in `text` component interacts with a `geometry` component for auto-sizing and anchoring. I think that's a nice feature so it's probably worth adding; in the meantime just use the `maxWidth` and `anchor`/`baseline` attributes to control it manually.

#### Changing the material

By default the text will render using a `MeshBasicMaterial` using the `color` property described in the table above. But you can also change the material to gain more advanced shader features such as physically-based lighting.

If you are using the `<a-troika-text>` _primitive_, you can assign it a [`material` component](https://aframe.io/docs/master/components/material.html) just as you would any other entity.

```html
<a-troika-text
  value="Hello!"
  material="shader: standard; metalness: 0.8;"
></a-troika-text>
```

If you are using the `troika-text="..."` _component_, you must instead give it a `troika-text-material="..."` attribute to distinguish the text material from the entity's main material. You can pass it anything supported by the built in [`material` component](https://aframe.io/docs/master/components/material.html).

```html
<a-entity
  troika-text="value: Hello!"
  troika-text-material="shader: standard; metalness: 0.8;"
></a-entity>
```

### Installation

#### Browser

Install and use by directly including the [browser files](dist):

```html
<head>
  <title>My A-Frame Scene</title>
  <script src="https://aframe.io/releases/1.0.3/aframe.min.js"></script>
  <script src="https://unpkg.com/aframe-troika-text/dist/aframe-troika-text.min.js"></script>
</head>

<body>
  <a-scene>
    <!-- As a component: -->
    <a-entity troika-text="value: Hello world!"></a-entity>
    
    <!-- As a primitive: -->
    <a-troika-text value="Hello world!"></a-troika-text>
  </a-scene>
</body>
```

#### npm

Install via npm:

```bash
npm install aframe-troika-text
```

Then require and use.

```js
require('aframe');
require('aframe-troika-text');
```
