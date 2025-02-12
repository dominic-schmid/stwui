<script lang="ts">
	import { setContext, onMount } from 'svelte/internal';
	import { twMerge } from 'tailwind-merge';
	import Placeholder from './Placeholder.svelte';
	import { get_current_component } from 'svelte/internal';
	import { forwardEventsBuilder, useActions, type ActionArray } from '../../actions';
	export let use: ActionArray = [];
	import { exclude } from '../../utils/exclude';
	const forwardEvents = forwardEventsBuilder(get_current_component());

	export let src: string | undefined = undefined;
	export let alt = 'avatar';
	export let shape: 'circle' | 'rounded' | 'square' = 'circle';

	let loaded = false;
	let failed = false;
	let loading = true;

	const defaultClass = 'inline-block absolute h-10 w-10';
	const containerDefaultClass = 'inline-block relative align-middle h-10 w-10';

	$: finalClass = twMerge(
		src ? defaultClass : false,

		shape === 'circle' ? 'rounded-full' : false,
		shape === 'rounded' ? 'rounded-md' : false,

		$$props.class
	);
	$: finalContainerClass = twMerge(
		src ? containerDefaultClass : false,

		shape === 'circle' ? 'rounded-full' : false,
		shape === 'rounded' ? 'rounded-md' : false,

		$$props.class
	);

	setContext('post-avatar-shape', shape);

	onMount(() => {
		if (src) {
			const image = new Image();
			image.src = src;
			loading = true;

			image.onload = () => {
				loading = false;
				loaded = true;
			};
			image.onerror = () => {
				loading = false;
				failed = true;
			};
		}
	});
</script>

<span
	class={finalContainerClass}
	use:useActions={use}
	use:forwardEvents
	{...exclude($$props, ['use', 'class'])}
>
	{#if loaded}
		<img class={finalClass} style={$$props.style} src={src || ''} {alt} />
	{:else if failed}
		{#if $$slots.placeholder}
			<slot name="placeholder" />
		{:else}
			<Placeholder />
		{/if}
	{:else if loading}
		<Placeholder loading />
	{/if}

	<slot name="indicator" />
</span>
