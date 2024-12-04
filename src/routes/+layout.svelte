<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<nav>
	<a href="/">Go Home Child</a>
</nav>
{@render children()}
