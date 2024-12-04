<script>
    import { fade, scale, fly } from 'svelte/transition';
    import { elasticOut } from 'svelte/easing';
    let munched = $state(0);
</script>

<main class="min-h-screen bg-gradient-to-br from-purple-100 to-indigo-100 py-16">
    <div class="max-w-2xl mx-auto px-4">
        <h1 
            class="text-4xl font-bold text-center text-indigo-900 mb-12"
            in:fly={{ y: -20, duration: 700, delay: 200 }}
        >
            Day Two
        </h1>
        
        <section 
            class="bg-white rounded-2xl shadow-xl p-8 text-center"
            in:scale={{ duration: 700, delay: 400, easing: elasticOut }}
        >
            <div class="h-24 flex justify-center items-center">
                {#key munched}
                    <h1 
                        class="text-6xl font-bold text-indigo-600"
                        in:scale={{ duration: 400, start: 0.5 }}
                        out:scale={{ duration: 400, start: 1.5 }}
                    >
                        {munched}
                    </h1>
                {/key}
            </div>
            
            <div 
                class="flex flex-wrap justify-center gap-4"
                in:fade={{ duration: 500, delay: 600 }}
            >
                <button 
                    type="button" 
                    class="px-6 py-3 bg-green-500 text-white font-semibold rounded-lg shadow-md hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-50 transition-all duration-200"
                    onclick={() => munched += 1}
                >
                    Add
                </button>
                
                <button 
                    type="button" 
                    class="px-6 py-3 bg-red-500 text-white font-semibold rounded-lg shadow-md hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-opacity-50 transition-all duration-200"
                    onclick={() => munched -= 1}
                >
                    Remove
                </button>
                
                <button 
                    type="button" 
                    class="px-6 py-3 bg-gray-500 text-white font-semibold rounded-lg shadow-md hover:bg-gray-600 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-opacity-50 transition-all duration-200"
                    onclick={() => munched = 0}
                >
                    Reset
                </button>
            </div>
        </section>
    </div>
</main>