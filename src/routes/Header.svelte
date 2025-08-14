<script lang="ts">
	let { name, fake_name = 'Wes' }: { name: string; fake_name?: string } = $props();
</script>

<h1>Hello, {name}</h1>
<h2>{name.replaceAll('t', 'X')}</h2>

{fake_name}

<style>
	h1 {
		color: blue;
	}
</style>
