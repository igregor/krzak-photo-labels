<script>
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import Logo from './Logo.svelte';
	import { mockData } from './mock-data';

	let items = mockData;
	let customText = 'klasa 1a';
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<div class="wrapper">
		{#each items as { email, id, name }}
			<div class="item-wrapper">
				<div class="left">
					<div class="picture"><Logo /></div>
				</div>
				<div class="right">
					<div class="id">
						<span class="small">numer zamówienia</span>
						{id}
					</div>
					<div class="name">{name.toLowerCase()}</div>
					<div class="custom">{customText}</div>
				</div>
			</div>
		{/each}
	</div>
</section>

<style>
	@page {
		size: A4 portrait;
		margin: 1mm 0mm;
	}
	.wrapper {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
	}
	.item-wrapper {
		aspect-ratio: 700/424; /* 70.0mm x 42.4mm https://neopak.pl/cms/szablony-etykiet */
		padding: 5%;
		border: 1px solid #ccc;
		display: flex;
		gap: 5%;
	}
	.left {
		width: 40%;
		display: flex;
		justify-content: center;
	}
	.right {
		/* flex-grow: 1; */
	}
	.picture {
		width: 150px;
	}
	.small {
		display: flex;
		font-size: 9px;
	}
	.id {
		flex-wrap: wrap;
		font-size: 32px;
	}
	.name {
		text-transform: capitalize;
		/* width: 20px; */
	}
	.custom {
		/* width: 20px; */
	}
</style>
