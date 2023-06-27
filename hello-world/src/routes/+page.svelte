

<script>

	import { onMount } from 'svelte';

	let todos = [];
 
	onMount(() => {   
		// Retrieve todos from local storage if they exist
		const storedTodos = localStorage.getItem('todos');
		if (storedTodos) {
			todos = JSON.parse(storedTodos);
		}
	});
 
	function saveTodos() {     
		localStorage.setItem('todos', JSON.stringify(todos));
	}


	let value1 = '';
	let line = '';

	function addTodo(val) {

		if (val === '') {
			alert('Please write something');
			return;
		}

		let toAdd = {
			data: val,
			done: false,
			line: ''
		};

		todos = [...todos, toAdd];
		value1 = '';
		saveTodos();  
		console.log(todos);
	}


	function removeTodo(ind) {
		todos = [...todos.slice(0, ind), ...todos.slice(ind + 1)];
		saveTodos();   
	}

	function changeCheckbox(done, index) {
		todos[index].done = !done;

		if (todos[index].done === true) {
			todos[index].line = 'line-through';
		} else {
			todos[index].line = '';
		}

		saveTodos(); 
	}

</script>


<main >

	<div>
		<div
			class="bg-[url('src/assets/image.jpg')]  w-2/5 place-content-center  mx-auto mt-16 my-16 py-10 bg-opacity-25 rounded-lg border-yellow-300 border-2 shadow-2xl " id="bg">
			<div class="text-center text-gray-400 text-4xl font-bold  ">TODO List</div>
			<br />
			<ul class="">
				{#each todos as todo, index (index)}
					<li class="flex flex-row py-2">
						<span class="basis-1/4 text-center">
							<input
								type="checkbox"
								id={index}
								on:click={() => {
									changeCheckbox(todo.done, index);
								}}
								checked={todo.done}
								class="accent-cyan-500 resize cursor-pointer h-4 w-4"
							/></span
						>
						<span class="basis-1/2 text-white ">
							&nbsp;&nbsp;&nbsp;&nbsp;<label
								class="text-lg {todos[index].line} decoration-red-700 decoration-solid decoration-4"
								for={index}
							>
								{todo.data}
							</label></span
						>
						<span class="basis-1/4 text-center"
							><button
								on:click={() => {
									removeTodo(index);
								}}
								class="bg-red-300/100 hover:bg-red-800 py-0.5 px-2 rounded-md text-white font-medium"
								>Remove</button
							></span
						>
					</li>
				{/each}
				</ul>

			<br />
			<div class="place-content-center">
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
					class="rounded-md h-8 ps-2"
					bind:value={value1}
				/>
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<button
					class="bg-sky-600/100 hover:bg-cyan-800 py-0.5 px-2 rounded-md text-white font-medium"
					on:click={() => addTodo(value1)}>Add Todo</button
				>
			</div>
		</div>
	</div>
</main>

<style>

</style>
