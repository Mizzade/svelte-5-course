<script lang="ts">
	let { name }: { name: string } = $props();
</script>

<h1>
	{name ? name : 'User'}'s Form
</h1>
