<script context="module" lang="ts">
	import { createEventDispatcher, onMount } from 'svelte';

	export type UpdateEditorDetail = {
		input: HTMLTextAreaElement;
		output: HTMLElement;
		content: HTMLElement;
		source: string;
	};

	export type UpdateEditor = CustomEvent<UpdateEditorDetail>;

	const range = (start: number, end: number) =>
		new Array(end - start + 1).fill(0).map((_, i) => i + start);
</script>

<script lang="ts">
	export { className as class };
	export let hideLines = false;
	export let lineCount = 1;
	export let source = '';
	/* -------- Styling -------- */
	export let background = null as string;
	export let fontFamily = 'monospace';
	export let fontSize = '1rem';
	export let height = '100%';
	export let lineHeight = 'initial';
	export let linesBg = null;
	export let linesColor = 'rgba(64, 64, 64, 0.9)';
	export let padding = '1rem';
	export let padx = '0rem';
	export let pady = '1rem';
	export let scrollThumb = '#222';
	export let scrollTrack = 'transparent';
	export let tabSize = 2;
	export let width = '100%';

	let input: HTMLTextAreaElement;
	let output: HTMLElement;
	let content: HTMLElement;
	let className = 'editor';
	let linesWidth = 0;

	const dispatch = createEventDispatcher<{ update: UpdateEditorDetail }>();

	function update() {
		output.textContent = source.at(-1) == '\n' ? source + ' ' : source;
		dispatch('update', { input, output, content, source });
		setTimeout(resizeInput, 0);
	}

	function resizeInput() {
		const { width, height } = content.getBoundingClientRect();
		input.style.height = `${height}px`;
		input.style.width = `${width}px`;
	}

	function ident(e: KeyboardEvent) {
		if (e.key == 'Tab') {
			e.preventDefault();
			const cursor = input.selectionEnd;
			const [before, after] = [source.slice(0, cursor), source.slice(cursor)];

			source = before + '\t' + after;
			update();
			setTimeout(() => (input.selectionEnd = cursor + 1), 0);
		}
	}

	onMount(update);

	$: inputClass = $$restProps['in-class'] ?? '';
	$: outputClass = $$restProps['out-class'] ?? '';
	$: contentClass = $$restProps['content-class'] ?? '';
	$: linesClass = $$restProps['lines-class'] ?? '';
	$: lineClass = $$restProps['line-class'] ?? '';
	$: lineCount = source ? source.split('\n').length : 1;
</script>

<svelte:window on:resize={resizeInput} />

<div
	class="editor {className}"
	style:--background={background}
	style:--fontFamily={fontFamily}
	style:--fontSize={fontSize}
	style:--height={height}
	style:--lineHeight={lineHeight}
	style:--linesBg={linesBg}
	style:--linesColor={linesColor}
	style:--marginLeft="calc({linesWidth}px + var(--padx))"
	style:--padding={padding}
	style:--padx={padx ?? padding ?? (hideLines ? '1rem' : '0rem')}
	style:--pady={pady ?? padding}
	style:--scrollThumb={scrollThumb}
	style:--scrollTrack={scrollTrack}
	style:--tabSize={tabSize}
	style:--width={width}
	class:with-bg={!!background}
	on:scroll
>
	{#if !hideLines}
		<ol class="lines {linesClass}" class:with-lines-bg={!!linesBg} bind:clientWidth={linesWidth}>
			{#each range(1, lineCount) as n}
				<li class={lineClass}>{n}</li>
			{/each}
		</ol>
	{/if}
	<textarea
		class="input {inputClass}"
		spellcheck={false}
		bind:this={input}
		bind:value={source}
		on:input={update}
		on:keydown={ident}
		on:input
		on:keypress
		on:keydown
		on:focus
		on:click
		on:mousedown
		on:mouseup
		on:mousemove
		on:mouseenter
		on:mouseleave
		on:mouseover
	/>
	<pre class="content {contentClass}" aria-hidden="true" bind:this={content}>
		<code class="output {outputClass}" bind:this={output} />
	</pre>
</div>

<style>
	.editor,
	.editor *,
	.editor *::after,
	.editor *::before {
		box-sizing: border-box;
		margin: 0;
	}

	.editor {
		/* Sizing */
		width: var(--width);
		height: var(--height);
		/* Other */
		position: relative;
		overflow: auto;
		scrollbar-width: thin;
		scrollbar-color: var(--scrollThumb) var(--scrollTrack);
	}
	/* Only apply a background when it is provided */
	.with-bg {
		background: var(--background);
	}
	/* Remove bg from child elements */
	.with-bg .input,
	.with-bg .output {
		background: transparent;
	}

	.lines {
		top: 0;
		left: 0;
		z-index: 10;
		position: sticky;
		color: var(--linesColor);
		padding: var(--pady) 0.825rem;
		background-color: inherit;
		min-height: 100%;
		height: max-content;
		min-width: 5ch;
		width: max-content;
		list-style-type: none;
		text-align: right;
	}

	.lines.with-lines-bg {
		background-color: var(--linesBg);
	}

	.input,
	.output {
		tab-size: var(--tabSize);
		padding: var(--pady) var(--padx);
		padding-left: var(--marginLeft);
	}

	.input,
	.output,
	.lines {
		font-family: var(--fontFamily);
		font-size: var(--fontSize);
		line-height: var(--lineHeight);
	}

	.input,
	.content,
	.output {
		min-width: 100%;
		min-height: 100%;
		overflow: hidden;
	}

	.input,
	.content {
		top: 0;
		left: 0;
		position: absolute;
		white-space: pre;
	}

	.content {
		pointer-events: none;
	}

	.input {
		z-index: 1;
		caret-color: #fff;
		color: transparent;
		background: transparent;
		resize: none;
		border: none;
		border-radius: 0;
		outline: none;
	}

	.editor::-webkit-scrollbar {
		width: 11px;
		height: 11px;
		cursor: default;
	}

	.editor::-webkit-scrollbar-thumb {
		background-color: var(--scrollThumb);
		border-radius: 99px;
	}

	.editor::-webkit-scrollbar-track {
		background-color: var(--scrollTrack);
	}

	.editor::-webkit-scrollbar-corner {
		background: var(--scrollTrack);
	}
</style>
