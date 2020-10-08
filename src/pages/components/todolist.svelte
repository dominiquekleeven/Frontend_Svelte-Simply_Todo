<script>
    import { onMount } from "svelte";
    import Todo from "./todo.svelte";
    import Icon from "fa-svelte";
    import { faPlus } from "@fortawesome/free-solid-svg-icons/faPlus";
    import { faTimesCircle } from "@fortawesome/free-solid-svg-icons/faTimesCircle";
    import { faTrash } from "@fortawesome/free-solid-svg-icons/faTrash";

    let usertoken = localStorage.getItem("token");
    let todos = [];
    let url = process.env.API_URL;
    let newlist = {};
    let newitem = {};

    let item_input = {
        visible: false,
    };

    onMount(async () => {
        console.log(
            "Welcome to Simply Todo - If you're reading this it probably means that you have your developer console open 🔥 "
        );

        await checkIfAuthorized();
        await fetchTodos();
    });

    async function showItemCreate(id) {
        item_input.visible = !item_input.visible;

        if (item_input.id === id) {
            item_input.id = 0;
        }

        item_input.id = id;
    }

    async function deleteTodo(id) {
        const response = await fetch(`${process.env.API_URL}/todos/${id}`, {
            method: "DELETE",
            headers: {
                "Content-Type": "application/json",
                Authorization: usertoken,
            },
        });

        handleResponseCode(response.status);
        const data = await response.json();
        if (data) {
            fetchTodos();
        }
    }

    async function createItem(e) {
        e.preventDefault();
        const response = await fetch(
            `${process.env.API_URL}/todos/${item_input.id}/items`,
            {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: usertoken,
                },
                body: JSON.stringify(newitem),
            }
        );

        handleResponseCode(response.status);
        const data = await response.json();
        if (data) {
            newitem = {};
            fetchTodos();
        }
    }

    async function createTodo(e) {
        e.preventDefault();
        const response = await fetch(`${process.env.API_URL}/todos/`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Authorization: usertoken,
            },
            body: JSON.stringify(newlist),
        });

        handleResponseCode(response.status);
        const data = await response.json();
        if (data) {
            newlist = {};
            fetchTodos();
        }
    }

    async function fetchTodos() {
        const response = await fetch(`${url}/todos`, {
            headers: { Authorization: usertoken },
        });

        handleResponseCode(response.status);

        const data = await response.json();
        todos = data.results;
    }

    function handleResponseCode(code) {
        if (code == 401 || code == 422) {
            localStorage.removeItem("token");
        }
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

     function orderItems(items) {
        var ordered = items.sort(function (a, b) {
            return a.guide - b.guide;
        });

        console.log(ordered)
        return ordered;
    }
</script>

<style>
    .custom-form {
        display: flex;
        flex-direction: row;
        font-size: 16px;
    }

    .custom-button {
    }

    form {
        display: flex;
        flex-direction: column;
    }
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
        font-size: 2.5em;
        margin-bottom: 0px;
        text-align: center;
    }
    button {
        color:  rgb(241, 174, 86);
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
        display: inline-flex;
        flex-direction: column;
    }

    .todo-name {
        color:  rgb(241, 174, 86);
        font-weight: 300;
        line-height: 2;
        display: flex;
        justify-content: space-between;
    }

    .icon-button {
        font-size: 20px !important;
        color: rgb(245, 245, 245);
        background-color: rgba(0, 0, 0, 0.671);
        padding: 10px;
        height: 40px;
        width: 40px;
        margin-right: 10px;
        border-radius: 0.5em;
        
    }

    .create-button {
        text-shadow: 1px 1px #252525;
        font-size: 24px;
    }

    .icon-button:hover {
        cursor: pointer;
        color:  rgb(241, 174, 86);
    }

    label {
        margin-top: 15px;
    }

    .item {
        display: inline-flex;
    }

    input {
        margin-top: 5px;
        border-radius: 0.25em;
    }

    input:focus {
        outline-color:  rgb(241, 174, 86);
    }

    .buttons {
        font-size: 18px;
        display: flex;
        justify-content: center;
    }
</style>

<div class="todolist">
    {#if todos.length <= 0}
        <h1>You don't have any Todo lists 😿</h1>
        <form on:submit={createTodo}>
            <label for="todolist">Create a new list</label>
            <input
                required
                bind:value={newlist.title}
                placeholder="My project"
                type="text" />
            <div class="buttons"><button>Create</button></div>
        </form>
    {:else}
        <h1>Todo List Collection ✔</h1>

        <form on:submit={createTodo}>
            <label style="font-size: 1.5em" for="todolist">Create a new list</label>
            <input
                required
                bind:value={newlist.title}
                placeholder="My project"
                type="text" />
            <div class="buttons"><button class="create-button">Create</button></div>
        </form>
    {/if}
    <hr />
    <ul>
        {#each todos as todo}
            <li class="todo">
                <span class="todo-name">{todo.title}
                    <span class="buttons">
                        {#if item_input.visible && item_input.id == todo.id}
                            <button
                                on:click={showItemCreate(todo.id)}
                                class="icon-button">
                                <Icon icon={faTimesCircle} /></button>
                        {:else}
                            <button
                                on:click={showItemCreate(todo.id)}
                                class="icon-button">
                                <Icon icon={faPlus} /></button>
                        {/if}

                        <button
                            on:click={deleteTodo(todo.id)}
                            class="icon-button">
                            <Icon icon={faTrash} /></button>
                    </span>
                </span>

                <ul class="todo-item-list">
                    {#if item_input.visible && item_input.id == todo.id}
                        <form class="custom-form" on:submit={createItem}>
                            <input
                                class="custom-input"
                                required
                                bind:value={newitem.name}
                                placeholder="New item"
                                type="text" />
                            <div class="buttons"><button>Create</button></div>
                        </form>
                    {/if}

                    {#each orderItems(todo.items) as item}
                        {#if item.done}
                            <span class="item" on:click={updateTodoItem(todo, item)}><Todo
                                    finished="true"
                                    description={item.name} /></span>
                        {:else}
                            <span class="item" on:click={updateTodoItem(todo, item)}><Todo
                                    finished="false"
                                    description={item.name} /></span>
                        {/if}
                    {/each}
                </ul>
            </li>
        {/each}
    </ul>
</div>
