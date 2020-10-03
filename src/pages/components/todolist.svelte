<script>
    import { onMount } from "svelte";
    import Todo from "./todo.svelte";
    import Icon from "fa-svelte";
    import { faPlus } from "@fortawesome/free-solid-svg-icons/faPlus";
    import { faTrash } from "@fortawesome/free-solid-svg-icons/faTrash";

    let plus = faPlus;
    let trash = faTrash;

    var usertoken = localStorage.getItem("token");
    let todos = [];
    let url = process.env.API_URL;

    onMount(async () => {
        console.log(
            "Welcome to Simply Todo - If you're reading this it probably means that you have your developer console open 🔥 "
        );

        await checkIfAuthorized();
        await fetchTodos();
    });

    async function fetchTodos() {
        const response = await fetch(`${url}/todos`, {
            headers: { Authorization: usertoken },
        });

        if (response.status == 401) {
            localStorage.removeItem("token");
        }
        const data = await response.json();
        todos = data.results;
    }

    async function updateTodoItem(todo, item) {
        item.done = !item.done;

        const response = await fetch(
            `${process.env.API_URL}/todos/${todo.id}/items/${item.id}`,
            {
                method: "PUT",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: usertoken,
                },
                body: JSON.stringify(item),
            }
        );

        const data = await response.json();

        if (data) {
            fetchTodos();
        }
    }

    async function checkIfAuthorized() {
        var token = localStorage.getItem("token");
        if (token == undefined) {
            document.location.href = "/login";
        }
    }
</script>

<style>
    .todolist {
        min-height: 20vh;
        /* box-shadow: 0px 3px 4px 2px rgba(0, 0, 0, 0.315); */
        padding: 25px;
        padding-top: 0px;
        display: inline-flex;
        flex-direction: column;
        border-radius: 0.35em;
        flex-wrap: nowrap;
        min-width: 25vw;
    }
    ul {
        padding: 0;
        margin: 0;
    }
    hr {
        width: 100%;
    }

    li {
        list-style: none;
        margin-bottom: 5px;
    }
    h1 {
        font-weight: 300;
        margin-bottom: 0px;
        text-align: center;
    }
    button {
        color: rgb(155, 37, 102);
        background-color: transparent;
        border: none;
    }
    button:hover {
        text-decoration: underline;
        cursor: pointer;
    }
    .todo {
        font-size: 2em;
    }

    .todo-item-list {
        margin-left: 25px;
        display: flex;
        flex-direction: column;
    }

    .todo-name {
        color: rgb(155, 37, 102);
        font-weight: 300;
        line-height: 2;
        display: flex;
        justify-content: space-between;
    }

    .icon-button {
        font-size: 18px !important;
        color: rgb(44, 44, 44);
    }

    .icon-button:hover {
        cursor: pointer;
        color: rgb(155, 37, 102);
    }
</style>

<div class="todolist">
    {#if todos.length <= 0}
        <h1>You don't have any Todo lists 😿</h1>
        <button>Click here to create a new todo list.</button>
    {:else}
        <h1>Todo List Collection ✔</h1>
        <button>Click here to create a new todo list </button>
    {/if}
    <hr />
    <ul>
        {#each todos as todo}
            <li class="todo">
                <span class="todo-name">{todo.title}
                    <span class="buttons">
                        <button class="icon-button">
                            <Icon icon={faPlus} /></button>

                        <button class="icon-button">
                            <Icon icon={faTrash} /></button>

                    </span>
                </span>
                <ul class="todo-item-list">
                    {#each todo.items as item}
                        {#if item.done}
                            <span on:click={updateTodoItem(todo, item)}><Todo
                                    finished="true"
                                    description={item.name} /></span>
                        {:else}
                            <span on:click={updateTodoItem(todo, item)}><Todo
                                    finished="false"
                                    description={item.name} /></span>
                        {/if}
                    {/each}
                </ul>
            </li>
        {/each}
    </ul>
</div>
