<script lang="ts">
	import Counter from './Counter.svelte';
	import XlsxDrop from './XlsxDrop.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import Logo from './Logo.svelte';
	import { mockData } from './mock-data';

	interface FileData {
		id: string;
		imie: string;
		nazwisko: string;
	}
	let itemsFromFile: FileData[] = [];
	// let items = mockData;
	let items = [];
	let customText = '';

	const chunkSize = 21;
	let pages: FileData[][] = [];

	$: {
		pages = [];

		for (let i = 0; i < itemsFromFile.length; i += chunkSize) {
			const chunk: FileData[] = itemsFromFile.slice(i, i + chunkSize);

			pages.push(chunk);
		}
		console.log('pages:', pages);
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<div class="no-print">
		<div class="max-w-sm mx-auto">
			<h1 class="text-2xl mb-5 mt-5 font-bold">Tworzyciel etykiet</h1>
			<XlsxDrop bind:itemsFromFile bind:customText />
			<div class="mt-10 mb-10">
				<h2>Podgląd</h2>
			</div>
		</div>
		<div></div>
	</div>

	<div class="pages-wrapper">
		{#each pages as page, i}
			<div class="wrapper mx-auto max-w-lg">
				<div class="inner-wrapper">
					{#each page as itemFromFile}
						<div class="item-wrapper">
							<div class="left">
								<div class="picture">
									<Logo />
								</div>
							</div>
							<div class="right">
								<div class="id">
									{itemFromFile.id}
								</div>
								{#if itemFromFile.custom}
									<div class="custom">{itemFromFile.custom}</div>
								{/if}
								<div class="name font-sm">
									{itemFromFile.imie.toLowerCase()}
									{itemFromFile.nazwisko.toLowerCase()}
								</div>
								{#if customText}
									<div class="custom">{customText}</div>
								{/if}
							</div>
						</div>
					{/each}
				</div>
			</div>
		{/each}
	</div>
</section>

<style>
	@page {
		size: A4 portrait;
		/* margin: 10mm 7mm; */
	}
	.pages-wrapper {
		margin: 0 auto;
		/* position: relative; */
		max-width: 500px;
		max-height: 700px;
		overflow: auto;
	}
	.Xwrapper {
		max-width: 900px;
		max-height: 600px;
		overflow: auto;
		/* outline: 1px dashed red; */
		/* width: 2100px; */
		/* height: 297px; */
		display: block;
		/* grid-template-columns: repeat(3, 1fr); */
		page-break-before: always;
		page-break-after: always;
	}
	.wrapper {
		width: 210mm;
		height: 297mm;
		height: 296mm;
		margin: 0 !important;
		/* position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		width: 100%;
		height: 100%; */
		outline: 1px dotted green;
		/* max-height: 600px; */
		overflow: auto;
		/* outline: 1px dashed red; */
		/* width: 2100px; */
		/* height: 297px; */
		/* display: grid; */
		/* grid-template-columns: repeat(3, 70mm); */
		page-break-before: always;
		padding: 15mm 7mm;
	}
	.inner-wrapper {
		/* width: 210mm; */
		/* max-height: 600px; */
		/* overflow: auto; */
		/* outline: 1px dashed red; */
		/* width: 2100px; */
		/* height: 297px; */
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		grid-template-rows: repeat(7, 144px);
		/* page-break-before: always; */

		/* margin: 10mm 7mm; */
		outline: 1px dotted red;
	}
	.Xitem-wrapper {
		aspect-ratio: 700/424; /* 70.0mm x 42.4mm https://neopak.pl/cms/szablony-etykiet */
		aspect-ratio: 730/424; /* najlepsze póki co - pierwsza strona igła! 70.0mm x 42.4mm https://neopak.pl/cms/szablony-etykiet */
		/* aktualne */
		aspect-ratio: 733/424;
		aspect-ratio: 700/423; /* 70.0mm x 42.4mm https://neopak.pl/cms/szablony-etykiet */
		padding: 5%;
		outline: 1px solid #ccc;
		display: flex;
		gap: 5%;
	}
	.item-wrapper {
		padding: 5%;
		/* width: calc(100 / 3); */
		/* float: left; */
		outline: 1px solid #ccc;
		overflow: hidden;
		display: flex;
		gap: 5%;
		/* width: 33.3%; */
		/* height: 42.4mm; */
	}
	.left {
		padding-top: 7px;
		width: 40%;
		display: flex;
		justify-content: center;
	}
	.right {
		/* flex-grow: 1; */
	}
	.picture {
		width: 100%;
	}
	.picture span {
		font-size: 10px;
	}
	/* .small {
		display: flex;
		font-size: 9px;
	} */
	.id {
		flex-wrap: wrap;
		font-size: 22px;
		padding-bottom: 5%;
		/* margin-bottom: 10%; */
		/* border-bottom: 1px solid #ccc; */
	}
	.name {
		text-transform: capitalize;
		font-size: 14px;
		/* width: 20px; */
	}
	.custom {
		font-size: 9px;
		/* width: 20px; */
	}

	@media print {
		.no-print,
		.no-print * {
			display: none !important;
		}
		.pages-wrapper,
		.wrapper {
			max-width: none;
			max-height: none;
			overflow: visible;
		}
		html,
		body {
			margin: 0 !important;
			padding: 0 !important;
		}
		* {
			outline: 0 !important;
		}
	}
</style>
