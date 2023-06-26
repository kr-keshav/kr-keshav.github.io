<!-- <script>

    let todos = ['',];

    function addTodo(){
        todos = [...todos,''];
    }

    function removeTodo(){

    }
    </script> 
    
    <main>
        {#each todos as todo,index}
            <input bind:value={todos[index]}>
            <br>

        {/each}

        <button on:click={addTodo}>Add Todo</button>
    </main> -->

<script>

	//     let todos = JSON.parse(localStorage.getItem('todos')) || [];  -----------------------------
	//     $: {
	// 	localStorage.setItem('todos', JSON.stringify(todos))
	// }

	//     let todos;                                                         ------------------------
	  // if(localStorage.getItem("todos")===null){
	  //   todos = [];
	  // }
	  // else {
	  //   todos = JSON.parse(localStorage.getItem("todos"));
	  // }

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

	//  $:localStorage.setItem('todos', JSON.stringify(todos));          -----------------------------------------

  // $:localStorage.setItem('todos', JSON.stringify(todos)); 
	let value1 = '';
	let line = '';

	function addTodo(val) {
		// todos.push(val)
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

	$: console.log(todos);

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

	/////no empty todo
</script>


<main >

	<div>
		<div
			class="w-2/5 mx-auto mt-16 mb-8 py-10 bg-opacity-25 rounded-lg border-yellow-300 border-2 shadow-2xl"
			id="bg"
		>
			<div class="text-center text-gray-400 text-4xl font-bold">TODO List</div>
			<br />
			<table class="table-auto border-separate border-spacing-4">
				{#each todos as todo, index (index)}
					<tr>
						<td class="text-center">
							<input
								type="checkbox"
								id={index}
								on:click={() => {
									changeCheckbox(todo.done, index);
								}}
								checked={todo.done}
								class="accent-cyan-500 resize cursor-pointer h-4 w-4"
							/></td
						>
						<td class="text-left text-white">
							&nbsp;&nbsp;&nbsp;&nbsp;<label
								class="text-lg {todos[index].line} decoration-red-700 decoration-solid decoration-4"
								for={index}
							>
								{todo.data}
							</label></td
						>
						<td class="text-center"
							><button
								on:click={() => {
									removeTodo(index);
								}}
								class="bg-red-300/100 hover:bg-red-800 py-0.5 px-2 rounded-md text-white font-medium"
								>Remove</button
							></td
						>
					</tr>
				{/each}
			</table>

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
	#bg {
		background-image: url('../assets/image.jpg');
		background-repeat: no-repeat;
	}

	table {
		width: 100%;
	}

	td {
		width: 100px;
	}
</style>
