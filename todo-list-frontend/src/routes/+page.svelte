<script lang="ts">
import { onMount } from "svelte";
import Sidebar from "../Sidebar/Sidebar.svelte";

let pingResponseMessage: string;
let pingResponseCount: number;

let todos: TodoItem[] = [];
let newTodoDescription = "";

								// Parameter
async function checkUncheckTodo(todo: TodoItem) {
	
}

async function getTodos() {
	const response = await fetch("http://localhost:3000/todo-item");
	const responseJson = await response.json();
	console.log(todos);
	todos = responseJson;
}

async function addTodoButtonclick() {
	const createTodoItemRequest: CreateTodoItemRequest = {
		description: newTodoDescription,
	};
	const createTodoItemRequestJson = JSON.stringify(createTodoItemRequest);

	const response = await fetch("http://localhost:3000/todo-item", {
		method: "POST", // This is a POST request, not the default GET
		headers: {
			"Content-Type": "application/json", // We are sending JSON
			mode: "no-cors", // no-cors, *cors, same-origin
		},
		body: createTodoItemRequestJson, // The JSON we are sending
	});

	console.log(response);

	// Once the new todo is added, re-ask server for to-do items (NOTE: This is flawed in a number of ways. Why?)
	getTodos();
}

// This runs when the page is loaded into the browser
onMount(() => {
	getTodos();
});

let active = false;

let sidebar = false;
function togglesidebar() {
	sidebar = !sidebar;
};

</script>
<div>
	<button class="sidebarbutton" on:click={togglesidebar}></button>
	{#if sidebar}
		<Sidebar></Sidebar>
	{/if}
</div>

<div class="poggers-title">
<h1>Poggers Checklist</h1></div>


<div class="checklist-container">
	<div class="checklist-title"><h2>Checklist Title</h2></div>
	{#each todos as todo}
	<div class="todo-item">[{todo.isDone ? "X" : " "}] {todo.description}</div>
{/each}	
		<input class="new-todo-input" name="new-todo-description" type="text" bind:value={newTodoDescription}/>
		<button class="todo-input-button" on:click={addTodoButtonclick}>+ Add New Task</button>
	</div>


<style>
	:global(body) {
		background-color: #FED9B7;
	}
	.poggers-title {
		text-align: center;
		color:#F07167;
		font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
		font-size: 30px;
		margin-top: -50px ;

	}
	.checklist-container {
		background-color: #FDFCDC;
		border-radius: 25px;
		width: 1000px;
		padding-bottom: 50px;
		padding-top: 20px;
		height: auto;
		margin-left: 17%;
		margin-bottom: 50%;
		margin-top: -20px;
	}
	.checklist-title {
		text-align: center;
		background-color: #F07167;
		width:1000px;
		color: #FFFFFF;
		font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
		font-size: 23px;
		box-shadow: 2px 2px 10px rgba(0, 0, 0,0.3);
	}
	.new-todo-input {
		width: 400px;
		height: 18px;
		padding: auto;
		margin-left: 10px;
		margin-right: 10px;
		background-color: #FDFCDC;
		border: none;
		border-bottom: solid 2px #0081A7;
		font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
		color:#0081A7;
		font-size: 18px;
	}
	.todo-input-button {
		background-color: rgba(0, 0, 0, 0);
		color: rgba(0, 129, 168, 0.3);
		border: none;
		font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
		font-size: 18px;
		text-align: center;
		transition-duration: 0.1s;
	}
	.todo-input-button:hover {
		transition-duration: 0.1s;
		color: #0081A7;
	}
	.todo-item {
		width: 200px;
		margin: 5px;
		padding: 10px;
		font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
		color:#0081A7;
		font-size: 18px;
	}

	.todo-item:hover {
		background-color: #f0706765;
		border-radius: 25px;
	}

	.sidebarbutton {
		background-color: #F07167;
		font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
		width: 100px;
		height: 70px;
		border: none;
		margin-left: -10px;
		margin-top: 11px;
		border-top-right-radius: 25px;
		border-bottom-right-radius: 25px;
		display: absolute;
		position: absolute;
	}

</style>