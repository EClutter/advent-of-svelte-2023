<script>
	let list = $state(null);
	let nice = $state(0);
	let naughty = $state(0);
	let niceList = $state(null);
	let naughtyList = $state(null);
	
	async function getData(){
		const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-one.json');
		list = await response.json();
		niceList = list.filter((item,index) => item.tally > 0)
		nice = niceList.reduce((sum, item) => sum + item.tally, 0)
		naughtyList = list.filter((item,index) => item.tally < 0)
		naughty = naughtyList.reduce((sum, item) => sum + item.tally, 0)
	}
	
	$effect(() => {
		getData();
	})
</script>
<section class="max-w-6xl mx-auto p-5">
    <div class="flex flex-col md:flex-row justify-around gap-8 mb-8">
        <div class="text-center p-4 rounded-lg flex-1 max-w-sm">
            <h1 class="text-2xl text-gray-800 m-0">Nice Total</h1>
            <span class="block text-4xl font-bold mt-2 text-green-500">{nice}</span>
        </div>
        <div class="text-center p-4 rounded-lg flex-1 max-w-sm">
            <h1 class="text-2xl text-gray-800 m-0">Naughty Total</h1>
            <span class="block text-4xl font-bold mt-2 text-red-500">{naughty}</span>
        </div>
    </div>
    
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        <div class="bg-gray-50 rounded-lg p-4">
            <h2 class="text-center mt-0 pb-2 border-b-2 border-gray-200">Naughty List</h2>
            <div class="max-h-[500px] overflow-y-auto">
                {#each list as item} 
                    {#if item.tally < 0}
                        <p class="flex justify-between p-2 my-1 rounded bg-white">
                            <span class="font-medium">{item.name}</span>
                            <span class="font-bold text-red-500">({item.tally})</span>
                        </p>
                    {/if}
                {/each}
            </div>
        </div>

        <div class="bg-gray-50 rounded-lg p-4">
            <h2 class="text-center mt-0 pb-2 border-b-2 border-gray-200">Nice List</h2>
            <div class="max-h-[500px] overflow-y-auto">
                {#each list as item} 
                    {#if item.tally >= 0}
                        <p class="flex justify-between p-2 my-1 rounded bg-white">
                            <span class="font-medium">{item.name}</span>
                            <span class="font-bold text-green-500">({item.tally})</span>
                        </p>
                    {/if}
                {/each}
            </div>
        </div>
    </div>
</section>