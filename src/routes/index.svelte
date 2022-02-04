<script lang="ts">
	import 'highlight.js/styles/github-dark.css';
	import hljs from 'highlight.js';
	import Editor from 'sveditor';
	import type { UpdateEditor } from 'sveditor';

	function highlight(e: UpdateEditor) {
		const { output } = e.detail;
		hljs.highlightElement(output);
	}
</script>

<main>
	<Editor on:update={highlight} class="hljs" out-class="javascript" fontSize="1.8rem" />
</main>

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
