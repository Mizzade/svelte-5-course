<script lang="ts">
	import { fly } from 'svelte/transition';
	import Header from './Header.svelte';

	let formState = $state({
		answers: {},
		name: '',
		birthday: '',
		step: 0,
		error: ''
	});

	$inspect(formState.step);

	const QUESTIONS = [
		{ question: "What's your name?", id: 'name', type: 'text' },
		{ question: "What's your birthday?", id: 'birthday', type: 'date' },
		{ question: "What's your favorite color?", id: 'color', type: 'color' }
	];

	function nextStep(id: string) {
		if (formState.answers[id]) {
			formState.step += 1;
			formState.error = '';
		} else {
			formState.error = 'Please fill out the form input';
		}
	}

	// Runs on mount
	$effect(() => {
		console.log('on mounted');

		return () => {
			// when unmounted or destroyed.
			// before effect re-runs
			// console.log('on unmounted');
		};
	});

	$effect(() => {
		// Will re-run when formState.step has changed
		// console.log('formState', formState.step);

		// DON'T create state based off other state, in effect.
		// Use #derived()
		return () => {
			// before effect re-runs
			// console.log('before formState re-runs', formState.step);
		};
	});
</script>

<Header name={formState.answers.name} />

<main>
	{#if formState.step >= QUESTIONS.length}
		<p>Thank you!</p>
	{:else}
		<p>Step: {formState.step + 1}</p>
	{/if}

	<!-- {@each QUESTIONS as question (question.id) } -->
	{#each QUESTIONS as { id, type, question }, index (id)}
		{#if formState.step === index}
			<div
				in:fly={{ x: 200, duration: 200, opacity: 0, delay: 200 }}
				out:fly={{ x: -200, duration: 200, opacity: 0 }}
			>
				{@render formStep({ question, id, type })}
			</div>
		{/if}
	{/each}

	{#if formState.error}
		<p class="error">{formState.error}</p>
	{/if}
</main>

{#snippet formStep({ question, id, type }: { question: string; id: string; type: string })}
	<article>
		<div>
			<label for={id}>{question}</label>
			<input {type} {id} bind:value={formState.answers[id]} />
		</div>
		<button onclick={() => nextStep(id)}>Next</button>
	</article>
{/snippet}

<style>
	.error {
		color: red;
	}
</style>
