<script>
	import { todoStore } from '$lib/test';
	$: ct = $todoStore.allTask.filter((e) => e.isCompleted === true);
	$: ut = $todoStore.allTask.filter((e) => e.isCompleted === false);
</script>

<form on:submit|preventDefault={() => todoStore.add($todoStore.task, $todoStore.isCompleted)}>
	<button disabled={$todoStore.isLoading} class="bg-blue-400">Submit</button>
	<input type="text" class="bg-transparent" bind:value={$todoStore.task} />
</form>
uncompleted<br />
{#await todoStore.get()}
	loading
{:then res}
	{#if $todoStore.allTask.length}
		<div class="flex flex-col  space-y-5">
			{#each ut as item}
				<div>
					<input
						type="checkbox"
						bind:checked={item.isCompleted}
						on:click={() => todoStore.toggle(item.id, item.isCompleted)}
						name=""
						id=""
					/>
					{item.task}
					<button on:click={() => todoStore.delete(item.id)}>Delete Todo</button>
				</div>
			{/each}
		</div>
	{:else}
		No current todos
	{/if}
{/await}
completed <br />
{#await ct}
	loading
{:then res}
	{#each ct as item}
		<div>
			<input
				type="checkbox"
				bind:checked={item.isCompleted}
				on:click={() => todoStore.toggle(item.id, item.isCompleted)}
				name=""
				id=""
			/>
			{item.task}
			<button on:click={() => todoStore.delete(item.id)}>Delete Todo</button>
		</div>
	{/each}
{/await}
