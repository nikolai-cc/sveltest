<script>
	import Window from './components/Window.svelte'

	let windows = [];
	let num = 20;
	let id = 0;
	let shadow = false;
	let blur = false;

	function addWindows(n) {
		for (let i = 0; i < n; i++) {
			windows = [...windows, { id:id }];
			id++;
		}
	}

	function rearrange(id) {
		windows = [...windows.filter(w => w.id !== id), { id: id }];
	}

	function remove(id) {
		windows = windows.filter(w => w.id !== id);
	}
</script>

<style>
	main {
		width: 100%;
		height: 100%;
		position: relative;
	}

	button {
		user-select: none;
		-webkit-user-select: none;
		touch-action: none;
	}
</style>

<main>
	Add: <input type="number" bind:value={num} min=0 max=1000> windows
	<button on:click={() => addWindows(num)}>
		Go (tot: {windows.length})
	</button>
	<button on:click={ () => shadow = !shadow }>
		Shadows are { shadow ? "on" : "off" }
	</button>
	<button on:click={ () => blur = !blur }>
		Motion blur is { blur ? "on" : "off" }
	</button>
	<p>
		{#each windows as window, i (window.id)}
			[{window.id}],
		{/each}
	</p>
	{#each windows as window, i (window.id)}
		<Window
		 	id={window.id}
			 on:mousedown={rearrange(window.id)}
			 on:touchstart={rearrange(window.id)}
			 on:mouseup={remove(window.id)}
			 on:touchend={remove(window.id)}
			effects={{
				shadow,
				blur,
			}}
		/>
	{/each}
</main>