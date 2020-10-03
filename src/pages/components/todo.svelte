<style>
    .todo-item {
        font-size: 0.7em;
        width: 100%;
    }

    .item {
        border: none;
        background-color: transparent;
        padding: 0;
        margin: 0;
        padding: 0;
        margin: 0;
        border-radius: 0;
    }
    li {
        list-style: none;
        margin-bottom: 5px;
    }

    .todo-done:hover {
        cursor: pointer;
    }

    .item:hover {
        cursor: pointer;
    }

    .todo-done {
        border: none;
        background-color: transparent;
        padding: 0;
        margin: 0;
        border-radius: 0;
        text-decoration: line-through;
    }
</style>

<script>
    export let description;
    export let finished = 'false';
</script>

{#if finished == 'true'}
<li class="todo-item"><button class="todo-done">{description}</button></li>
{:else}
<li class="todo-item"><button class="item">{description}</button></li>
{/if}

