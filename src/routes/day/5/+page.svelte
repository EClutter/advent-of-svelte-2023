<script>
	import { fade, slide } from 'svelte/transition';

	let loading = $state(true);
	let tasks = $state(null);
	const elfs = $derived(tasks ? Array.from(new Set(tasks.map((task) => task.elf))) : []);
	let activeTasks = $state(null);
	let selectedElf = $state(null);
	let selectedElfMinutesWorked = $derived(
		activeTasks ? activeTasks.reduce((sum, item) => sum + item.minutesTaken, 0) : 0
	);

	let selectedElfAverageTime = $derived(
		activeTasks && activeTasks.length > 0
			? Math.round(selectedElfMinutesWorked / activeTasks.length)
			: 0
	);

	let selectedElfTaskCount = $derived(activeTasks ? activeTasks.length : 0);
	function filterByElf(elf) {
		selectedElf = elf;
		activeTasks = tasks.filter((item) => elf === item.elf);
	}

	async function loadData() {
		const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-five.json');
		const json = await response.json();
		tasks = json;
		loading = false;
	}

	$effect(() => {
		if (loading) {
			loadData();
		}
	});
</script>

<section class="mx-auto max-w-7xl p-6">
	<div class="rounded-xl bg-white p-6 shadow-lg">
		<h1 class="mb-8 text-3xl font-bold text-gray-800">Elf Task Manager 🎄</h1>

		<div class="mb-8">
			<h2 class="mb-4 text-xl font-semibold text-gray-700">Select an Elf</h2>
			<div class="grid grid-cols-2 gap-3 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6">
				{#each elfs as elf}
					<button
						type="button"
						class="rounded-lg px-4 py-2 text-sm font-medium transition-all duration-200
                            {selectedElf === elf
							? 'bg-green-500 text-white shadow-md'
							: 'bg-gray-100 text-gray-700 hover:bg-green-100'}"
						onclick={() => filterByElf(elf)}
					>
						Elf {elf}
					</button>
				{/each}
			</div>
		</div>

		{#if activeTasks}
			<div class="rounded-xl bg-gray-50 p-6">
				<h2 class="mb-4 text-xl font-semibold text-gray-700">
					{selectedElf ? `Tasks for Elf ${selectedElf}` : 'Select an elf to view tasks'}
				</h2>
				<div class="mb-6 grid grid-cols-3 gap-4">
					<div class="rounded-lg bg-white p-4 text-center">
						<h3 class="text-sm text-gray-500">Total Time</h3>
						<p class="text-2xl font-bold text-green-600">{selectedElfMinutesWorked} mins</p>
					</div>
					<div class="rounded-lg bg-white p-4 text-center">
						<h3 class="text-sm text-gray-500">Average Time</h3>
						<p class="text-2xl font-bold text-blue-600">{selectedElfAverageTime} mins</p>
					</div>
					<div class="rounded-lg bg-white p-4 text-center">
						<h3 class="text-sm text-gray-500">Tasks</h3>
						<p class="text-2xl font-bold text-purple-600">{selectedElfTaskCount}</p>
					</div>
				</div>
				<div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
					{#each activeTasks as task}
						<div
							class="rounded-lg bg-white p-4 shadow transition-all duration-200 hover:shadow-md"
							in:fade={{ duration: 200 }}
						>
							<div class="mb-3 flex items-center justify-between">
								<span class="text-lg font-semibold text-gray-800">Task {task.task}</span>
								<span class="rounded bg-green-100 px-2 py-1 text-sm font-medium text-green-600">
									{task.minutesTaken} mins
								</span>
							</div>
							<div class="text-sm text-gray-500">
								{new Date(task.date).toLocaleDateString('en-US', {
									year: 'numeric',
									month: 'long',
									day: 'numeric'
								})}
							</div>
						</div>
					{/each}
				</div>
			</div>
		{:else if !loading}
			<div class="py-8 text-center text-gray-500">Select an elf to view their tasks</div>
		{/if}
	</div>
</section>
