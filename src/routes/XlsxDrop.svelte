<script lang="ts">
	import { read, utils } from 'xlsx';

	export let itemsFromFile;
	export let customText;
	export let isDraggingOver: boolean = false;

	const readExcel = async (file: any) => {
		console.log(file);
		const fileReader = await new FileReader();
		fileReader.readAsArrayBuffer(file);

		fileReader.onload = (e: any) => {
			const bufferArray = e?.target.result;
			const wb = read(bufferArray, { type: 'buffer' });
			const wsname = wb.SheetNames[0];
			const ws = wb.Sheets[wsname];

			const data = utils.sheet_to_json(ws, { raw: true });
			const fileName = file.name.split('.')[0];

			console.log(data);

			itemsFromFile = data;
		};
	};
</script>

<div class="max-w-sm mx-auto">
	<div class="mb-5">
		<label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
			>Dodatkowy tekst na etykiecie</label
		>
		<input
			type="email"
			id="email"
			class="bg-gray-50 border border-gray-300 text-gray-900 text-sm
			rounded-lg focus:ring-blue-500 focus:border-blue-500
			block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
			placeholder="np. Klasa 1 c"
			required
			on:change={(e) => {
				customText = e.target.value;
			}}
		/>
	</div>

	<div class=" relative">
		<div
			class="flex flex-col items-center justify-center w-full h-64 border-2 border-gray-300 rounded-lg cursor-pointer bg-gray-50 dark:hover:bg-gray-800 dark:bg-gray-700 hover:bg-gray-100 dark:border-gray-600 dark:hover:border-gray-500 dark:hover:bg-gray-600
			{isDraggingOver ? 'border-solid' : 'border-dashed'}"
		>
			<div class="flex flex-col items-center justify-center pt-5 pb-6">
				<svg
					class="w-8 h-8 mb-4 text-gray-500 dark:text-gray-400"
					aria-hidden="true"
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 20 16"
				>
					<path
						stroke="currentColor"
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M13 13h3a3 3 0 0 0 0-6h-.025A5.56 5.56 0 0 0 16 6.5 5.5 5.5 0 0 0 5.207 5.021C5.137 5.017 5.071 5 5 5a4 4 0 0 0 0 8h2.167M10 15V6m0 0L8 8m2-2 2 2"
					/>
				</svg>
				<p class="mb-2 text-sm text-gray-500 dark:text-gray-400">
					<span class="font-semibold">Kliknij aby dodać plik</span> lub upuść plik
				</p>
				<p class="text-xs text-gray-500 dark:text-gray-400">
					tylko pliki *.xlsx (nagłówki: id, imie, nazwisko)
				</p>
			</div>
		</div>
		<input
			class="absolute bottom-0 left-0 top-0 right-0 opacity-0"
			type="file"
			on:dragover={() => {
				isDraggingOver = true;
			}}
			on:dragleave={() => {
				isDraggingOver = false;
			}}
			on:change={(e) => {
				const file = e.target.files[0];
				readExcel(file);
			}}
		/>
	</div>
</div>

<style>
</style>
