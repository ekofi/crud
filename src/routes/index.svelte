<script>
	import { todoStore } from '$lib/test';
</script>

<form on:submit|preventDefault={() => todoStore.add($todoStore.task, $todoStore.isCompleted)}>
	<button disabled={$todoStore.isLoading} class="bg-blue-400">Submit</button>
	<input type="text" class="bg-transparent" bind:value={$todoStore.task} />
</form>
{$todoStore.isLoading}
{#await todoStore.get()}
	loading
{:then res}
	{#if $todoStore.allTask.length}
		<div class="flex flex-col  space-y-5">
			{#each $todoStore.allTask as item}
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
