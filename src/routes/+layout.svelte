<script lang="ts">
	// import { Router } from 'svelte-navigator';
	import '../app.css';
	
	let { children } = $props();
</script>
<!-- svelte-ignore slot_element_deprecated -->
<slot />
<!-- {@render children()} -->
