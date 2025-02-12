<script lang="ts">
	import { Autocomplete, Card, Col } from '../../lib';
	import {
		example,
		allowNonOptionExample,
		props,
		slots,
		labelSlots,
		optionsSlots,
		optionProps,
		emptyOptionSlots,
		iconProps
	} from './examples';
	import { PropsTable, SlotsTable, CodeBlock } from '../../docs';
	import { email } from '../../docs/icons';

	let value1: string;
	let value2: string;
	let value3: string;
	let value4 = 'I am not in the options!';

	let options1 = ['Option 1', 'Option 2', 'Option 3'];
	let options2 = ['Option 1', 'Option 2', 'Option 3'];
	let options3 = ['Option 1', 'Option 2', 'Option 3'];
	let options4 = ['Option 1', 'Option 2', 'Option 3'];

	let filtered1 = ['Option 1', 'Option 2', 'Option 3'];
	let filtered2 = ['Option 1', 'Option 2', 'Option 3'];
	let filtered3 = ['Option 1', 'Option 2', 'Option 3'];
	let filtered4 = ['Option 1', 'Option 2', 'Option 3'];

	function filter1(e: Event) {
		const target = e.target as HTMLInputElement;
		filtered1 = options1.filter((opt) => opt.toLowerCase().includes(target.value.toLowerCase()));
	}

	function filter2(e: Event) {
		const target = e.target as HTMLInputElement;
		filtered2 = options2.filter((opt) => opt.toLowerCase().includes(target.value.toLowerCase()));
	}

	function filter3(e: Event) {
		const target = e.target as HTMLInputElement;
		filtered3 = options3.filter((opt) => opt.toLowerCase().includes(target.value.toLowerCase()));
	}

	function filter4(e: Event) {
		const target = e.target as HTMLInputElement;
		filtered4 = options4.filter((opt) => opt.toLowerCase().includes(target.value.toLowerCase()));
	}

	function filterOptions(option: string, key: 1 | 2 | 3 | 4) {
		if (key === 1) {
			if (option) {
				filtered1 = options1.filter((opt) => opt.toLowerCase().includes(option.toLowerCase()));
			} else {
				filtered1 = options1;
			}
		} else if (key === 2) {
			if (option) {
				filtered2 = options2.filter((opt) => opt.toLowerCase().includes(option.toLowerCase()));
			} else {
				filtered2 = options2;
			}
		} else if (key === 3) {
			if (option) {
				filtered3 = options3.filter((opt) => opt.toLowerCase().includes(option.toLowerCase()));
			} else {
				filtered3 = options3;
			}
		} else {
			if (option) {
				filtered4 = options4.filter((opt) => opt.toLowerCase().includes(option.toLowerCase()));
			} else {
				filtered4 = options4;
			}
		}
	}

	$: filterOptions(value1, 1);
	$: filterOptions(value2, 2);
	$: filterOptions(value3, 3);
	$: filterOptions(value4, 4);

	let error: string | undefined = "You're doing it wrong!";
	$: if (value3 && value3.length > 0) {
		error = undefined;
	} else {
		error = "You're doing it wrong!";
	}
</script>

<Col class="col-24 md:col-12">
	<Card bordered={false}>
		<Card.Header slot="header">Basic</Card.Header>
		<Card.Content slot="content" class="p-4">
			<Autocomplete
				name="select-1"
				placeholder="Basic"
				bind:value={value1}
				on:input={filter1}
				options={options1}
			>
				<Autocomplete.Options slot="options">
					{#if filtered1.length > 0}
						{#each filtered1 as option}
							<Autocomplete.Options.Option {option} />
						{/each}
					{:else}
						<Autocomplete.Options.EmptyOption />
					{/if}
				</Autocomplete.Options>
			</Autocomplete>
			<br />
			<Autocomplete name="select-2" bind:value={value2} on:input={filter2} options={options2}>
				<Autocomplete.Label slot="label">Label</Autocomplete.Label>
				<Autocomplete.Leading slot="leading" data={email} />
				<Autocomplete.Options slot="options">
					{#if filtered2.length > 0}
						{#each filtered2 as option}
							<Autocomplete.Options.Option {option} />
						{/each}
					{:else}
						<Autocomplete.Options.EmptyOption />
					{/if}
				</Autocomplete.Options>
			</Autocomplete>
			<br />
			<Autocomplete
				name="select-3"
				{error}
				bind:value={value3}
				on:input={filter3}
				options={options3}
			>
				<Autocomplete.Label slot="label">Label</Autocomplete.Label>
				<Autocomplete.Leading slot="leading" data={email} />
				<Autocomplete.Options slot="options">
					{#if filtered3.length > 0}
						{#each filtered3 as option}
							<Autocomplete.Options.Option {option} />
						{/each}
					{:else}
						<Autocomplete.Options.EmptyOption />
					{/if}
				</Autocomplete.Options>
			</Autocomplete>

			<br />

			<CodeBlock language="svelte" code={example} />
		</Card.Content>
	</Card>
</Col>

<Col class="col-24 md:col-12">
	<Card bordered={false}>
		<Card.Header slot="header">Any Value</Card.Header>
		<Card.Content slot="content" class="p-4">
			<Autocomplete
				name="select-4"
				placeholder="Basic"
				bind:value={value4}
				on:input={filter4}
				options={options4}
				allowNonListValue
			>
				<Autocomplete.Options slot="options">
					{#if filtered4.length > 0}
						{#each filtered4 as option}
							<Autocomplete.Options.Option {option} />
						{/each}
					{:else}
						<Autocomplete.Options.EmptyOption />
					{/if}
				</Autocomplete.Options>
			</Autocomplete>

			<br />

			<CodeBlock language="svelte" code={allowNonOptionExample} />
		</Card.Content>
	</Card>
</Col>

<Col class="col-24">
	<PropsTable component="Autocomplete" {props} />
</Col>

<Col class="col-24">
	<SlotsTable component="Autocomplete" {slots} />
</Col>

<Col class="col-24">
	<SlotsTable component="Autocomplete.Label" slots={labelSlots} />
</Col>

<Col class="col-24">
	<PropsTable component="Autocomplete.Leading" props={iconProps} />
</Col>

<Col class="col-24">
	<SlotsTable component="Autocomplete.Options" slots={optionsSlots} />
</Col>

<Col class="col-24">
	<PropsTable component="Autocomplete.Options.Option" props={optionProps} />
</Col>

<Col class="col-24">
	<SlotsTable component="Autocomplete.Options.EmptyOption" slots={emptyOptionSlots} />
</Col>
