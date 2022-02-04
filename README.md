# sveditor

A simple code editor component for svelte with support for syntax highlighting.

> Note: this project is still in development and bugs may occur

## Installation

Install with npm

```bash
npm install sveditor
```

## Basic Usage

```svelte
<script>
   import Editor from 'sveditor'

   let source = '<div>Hello, world!</div>'

   function update(event) {
      const { input, output } = event.detail
      highlight(output)
   }
</script>

<Editor
   bind:source
   fontFamily="monospace"
   fontSize="1.8rem"
   width="100%"
   height="100%"
   background="#333"
   class="my-editor"
   on:update={update}
/>
```

By default, the component will not attempt to highlight any of
the source code. Instead, sveditor allows the user to manually
provide the way which the text is highlighted. This flexibility
allows to integrate the component with various syntax 
highlighting libraries, like [Prism](https://prismjs.com/) or 
[Highlight.js](https://highlightjs.org/). You can even create
your own syntax highlighting engine on top of it.

## Example using Highlight.js

Lets create a simple **javascript** code editor with Highlight.js

Assuming you already have a svelte project created, 
install the dependencies:

```bash
npm i highlight.js sveditor
```

In the main svelte file:

```svelte
<script>
   import Editor from 'sveditor'
   import hljs from 'highlight.js'
   /* Using github dark style */
   import 'highlight.js/styles/github-dark.css';
   // This function will be called
   // everytime the editor updates
   function highlight(e) {
      // e.detail.output is the <code> element
      // that displays the processed text
      hljs.highlightElement(e.detail.output);
   }
</script>

<!-- 
   The out-class attr will be passed to the <code> element.
   We use here because highlight.js identifies the languages 
   by the class of the <code> element
-->

<Editor 
   class="hljs"
   out-class="javascript"
   fontSize="1.8rem"
   on:update={highlight}
/>

<style>
	:global(body) {
		margin: 0;
		display: flex;
	}

	main {
		width: 100vw;
		height: 100vh;
	}
</style>
```

### Result

![Preview](./preview.gif)


## License

Copyright © 2022 Jorge Brito

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.