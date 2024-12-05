<script>
	import { fade, fly, slide, scale } from 'svelte/transition';
	import { elasticOut } from 'svelte/easing';

	let items = $state(null);
	let weight = $state(0);
	let message = $state('Start Adding to the sleigh...');
	let loading = $state(true);
	let sleighItems = $state([]);
	let deliveredCount = $state(0);
	let idCounter = $state(0);

	async function getData() {
		const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-three.json');
		items = await response.json();
		// Add unique IDs to each item
		items = items.map((item) => ({ ...item, id: idCounter++ }));
	}

	$effect(() => {
		if (loading) {
			getData();
		}
	});

	function addWeight(item) {
		const newWeight = parseFloat((weight + item.weight).toFixed(2));
		if (newWeight >= 100) {
			message = '🚫 The sleigh would be overweight! Please remove items first.';
		} else {
			weight = newWeight;
			message = `🎁 Added ${item.name} (${item.weight}kg) to the sleigh`;
			sleighItems = [...sleighItems, item];
			items = items.filter((n) => n !== item);
		}
	}

	function removePresent(item) {
		weight = parseFloat((weight - item.weight).toFixed(2));
		message = `❌ Removed ${item.name} (${item.weight}kg) from the sleigh`;
		items = [...items, item];
		sleighItems = sleighItems.filter((n) => n !== item);
	}

	function deliverPresent(item) {
		weight = parseFloat((weight - item.weight).toFixed(2));
		message = `🎄 Delivered ${item.name} to its destination!`;
		sleighItems = sleighItems.filter((n) => n !== item);
		deliveredCount++;
	}
</script>

<section class="mx-auto max-w-6xl p-6" in:fade={{ duration: 300 }}>
    <div class="bg-white rounded-xl shadow-lg p-6 mb-8">
        <div class="flex justify-between items-center mb-6">
            <h1 class="text-lg font-medium">Status: 
                <span class="text-indigo-600 font-normal">{message}</span>
            </h1>
            <div class="text-green-600 font-medium">
                🎄 Delivered: {deliveredCount}
            </div>
        </div>
        
        <div class="bg-indigo-50 rounded-lg p-4 mb-6">
            <div class="flex items-center justify-between">
                <span class="text-indigo-900 font-medium">Sleigh Capacity:</span>
                <div class="flex items-center gap-2">
                    <div class="w-48 h-4 bg-white rounded-full overflow-hidden">
                        <div 
                            class="h-full bg-gradient-to-r from-green-500 to-yellow-500 transition-all duration-300"
                            style="width: {weight}%"
                        ></div>
                    </div>
                    <span class="text-indigo-900 font-bold">{weight}/100 kg</span>
                </div>
            </div>
        </div>
	<div class="grid gap-8 md:grid-cols-2">
		<div class="space-y-4">
			<h2 class="mb-4 text-xl font-semibold text-gray-800">Available Items</h2>
			<div class="space-y-3">
				{#each items as item (item.id)}
					<div
						class="flex items-center justify-between rounded-lg bg-gray-50 p-3 transition-colors hover:bg-gray-100"
						in:fly={{ x: -100, duration: 300 }}
						out:fly={{ x: 100, duration: 300 }}
					>
						<div>
							<p class="font-medium text-gray-800">{item.name}</p>
							<p class="text-sm text-gray-500">{item.weight}kg</p>
						</div>
						<button
							type="button"
							class="rounded-lg bg-green-500 px-4 py-2 text-white transition-colors hover:bg-green-600"
							onclick={() => addWeight(item)}
						>
							Add
						</button>
					</div>
				{/each}
			</div>
		</div>

		<div class="space-y-4">
			<h2 class="mb-4 text-xl font-semibold text-gray-800">Items in Sleigh</h2>
			<div class="space-y-3">
				{#each sleighItems as item (item.id)}
					<div
						class="flex items-center justify-between rounded-lg bg-gray-50 p-3 transition-colors hover:bg-gray-100"
						in:fly={{ x: 100, duration: 300 }}
						out:fly={{ x: -100, duration: 300 }}
					>
						<div>
							<p class="font-medium text-gray-800">{item.name}</p>
							<p class="text-sm text-gray-500">{item.weight}kg</p>
						</div>
						<div class="flex gap-2">
							<button
								type="button"
								class="rounded-lg bg-green-500 px-4 py-2 text-white transition-colors hover:bg-green-600"
								onclick={() => deliverPresent(item)}
							>
								Deliver
							</button>
							<button
								type="button"
								class="rounded-lg bg-red-500 px-4 py-2 text-white transition-colors hover:bg-red-600"
								onclick={() => removePresent(item)}
							>
								Remove
							</button>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</div>
</section>
