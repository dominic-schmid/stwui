<script lang="ts">
	import { trending_down, trending_neutral, trending_up } from '$lib/icons';
	import { twMerge } from 'tailwind-merge';
	import Badge from '../badge';
	import TrendIcon from './TrendIcon.svelte';

	export let trend: 'up' | 'down' | 'neutral' | undefined = undefined;
	export let showIcon = true;

	const defaultClass = 'bg-opacity-20 dark:bg-opacity-20';
	const trendUpClass =
		'bg-success-background dark:bg-dark-success-background text-success-content dark:text-dark-success-content';
	const trendDownClass =
		'bg-error-background dark:bg-dark-error-background text-error-content dark:text-dark-error-content';
	const trendEvenClass =
		'bg-info-background dark:bg-dark-info-background text-info-content dark:text-dark-info-content';

	$: finalClass = twMerge(
		defaultClass,

		trend === 'up' ? trendUpClass : false,
		trend === 'down' ? trendDownClass : false,
		trend === 'neutral' ? trendEvenClass : false,

		$$props.class
	);
</script>

<Badge class={finalClass} style={$$props.style}>
	{#if showIcon}
		{#if $$slots.icon}
			<span class="mr-1">
				<slot name="icon" />
			</span>
		{:else}
			<span class="mr-1">
				{#if trend === 'up'}
					<TrendIcon data={trending_up} />
				{:else if trend === 'down'}
					<TrendIcon data={trending_down} />
				{:else}
					<TrendIcon data={trending_neutral} />
				{/if}
			</span>
		{/if}
	{/if}
	<span><slot /></span>
</Badge>
