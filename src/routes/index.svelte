<script lang="ts">
	import 'highlight.js/styles/github-dark.css';
	import hljs from 'highlight.js';
	import Editor from 'sveditor';
	import type { UpdateEditor } from 'sveditor';

	async function highlight(e: UpdateEditor) {
		const { output } = e.detail;
		hljs.highlightElement(output);
	}

	let lang = 'javascript';

	setTimeout(() => {
		lang = 'html';
	}, 2000);
</script>

<main>
	<Editor on:update={highlight} class="hljs" outputClass={lang} fontSize="1.8rem" />
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
