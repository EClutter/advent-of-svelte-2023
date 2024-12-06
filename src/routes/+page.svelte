<script>
	let TOTAL_DAYS = 25;
	let COMPLETED_DAYS = 5;
	const days = Array.from({ length: TOTAL_DAYS }, (_, i) => i);
</script>

<div class="py-8">
	<h1 class="mb-12 text-center text-4xl font-bold text-slate-800">Svelte Advent Calendar 2023</h1>

	<div
		class="mx-auto grid max-w-3xl grid-cols-2 gap-4 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5"
	>
		{#each days as day}
			<a href={`/day/${day + 1}`} class="transform transition-all duration-200 hover:scale-105">
				<button
					type="button"
					class={`w-full rounded-xl bg-gradient-to-br px-6 py-4 ${day < COMPLETED_DAYS ? 'from-yellow-500 to-red-600' : 'from-indigo-500 to-purple-600'} font-medium text-white shadow-lg transition-all duration-200 hover:from-indigo-600 hover:to-purple-700 hover:shadow-xl focus:outline-none focus:ring-4 focus:ring-indigo-300`}
				>
					Day {day + 1}
				</button>
			</a>
		{/each}
	</div>
</div>
