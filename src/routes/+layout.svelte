<script lang="ts">
	import { Router } from 'svelte-navigator';
	import '../app.css';
	
	let { children } = $props();
</script>
<Router>
	<slot />
</Router>
<!-- {@render children()} -->
