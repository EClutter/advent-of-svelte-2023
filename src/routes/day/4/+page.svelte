<script>
    import { fade, slide } from 'svelte/transition';
    
	let heartRates = $state([]);
	let intervalId;
    let idCounter = $state(0);

	let average = $derived.by(() => {
		if (heartRates.length > 10) {
			let total = 0;
			for (const n of heartRates) {
				total += n.value;
			}
			return Math.ceil((total / heartRates.length) * 100) / 100;
		} else {
			return 0;
		}
	});

	async function getHeartRate() {
		try {
			const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-four.json');
			const data = await response.json();
			heartRates = [...heartRates, { id: idCounter++, value: data.heartRate }];
            if (heartRates.length > 20) {
                heartRates = heartRates.slice(-20);
            }
		} catch (error) {
			console.error('Error fetching heart rate:', error);
		}
	}

	$effect(() => {
		intervalId = setInterval(getHeartRate, 1000);

		return () => {
			if (intervalId) clearInterval(intervalId);
		};
	});

    function getHeartRateColor(rate) {
        if (rate < 60) return 'text-blue-500';
        if (rate > 100) return 'text-red-500';
        return 'text-green-500';
    }
</script>

<section class="max-w-4xl mx-auto p-6">
    <div class="bg-white rounded-xl shadow-lg p-6">
        <div class="flex items-center justify-between mb-8">
            <h2 class="text-2xl font-bold text-gray-800">Heart Rate Monitor</h2>
            <div class="bg-gray-50 rounded-lg p-4 shadow-sm">
                <p class="text-sm text-gray-500">Average Rate</p>
                <p class="text-2xl font-bold text-indigo-600">{average} BPM</p>
            </div>
        </div>

        <div class="grid grid-cols-4 gap-4 mb-6">
            {#each heartRates.slice(-4) as { id, value } (id)}
                <div 
                    class="bg-gray-50 rounded-lg p-4 text-center"
                    in:fade={{ duration: 200 }}
                >
                    <p class={`text-3xl font-bold ${getHeartRateColor(value)}`}>
                        {value}
                    </p>
                    <p class="text-sm text-gray-500">BPM</p>
                </div>
            {/each}
        </div>
    </div>
</section>