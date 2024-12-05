<script>
	let COMPLETED_DAYS = 2;
    const days = Array.from({ length: COMPLETED_DAYS }, (_, i) => i);
</script>

<div class="py-8">
    <h1 class="text-4xl font-bold text-center mb-12 text-slate-800">
        Svelte Advent Calendar 2023
    </h1>
    
    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4 max-w-3xl mx-auto">
        {#each days as day}
            <a 
                href={`/day/${day + 1}`}
                class="transform transition-all duration-200 hover:scale-105"
            >
                <button
                    type="button"
                    class="w-full py-4 px-6 rounded-xl bg-gradient-to-br from-indigo-500 to-purple-600 text-white font-medium shadow-lg hover:shadow-xl hover:from-indigo-600 hover:to-purple-700 focus:ring-4 focus:ring-indigo-300 focus:outline-none transition-all duration-200"
                >
                    Day {day + 1}
                </button>
            </a>	
        {/each}
    </div>
</div>