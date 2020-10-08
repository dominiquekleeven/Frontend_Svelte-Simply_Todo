<style>
    .todo-item {
        font-size: 0.9em;
        width: 100%;
    }

    .item {
        border: none;
        background-color: transparent;
        padding: 0;
        margin: 0;
        padding: 0;
        font-weight: 250;
        text-shadow: 1px 1px #252525;
        margin: 0;
        border-radius: 0;
        color: rgb(235, 235, 235);
    }
    li {
        list-style: none;
        margin-bottom: 5px;
    }

    .todo-done:hover {
        cursor: pointer;
    }

    .item:active{
        background-color: transparent !important;
    }

    .done:active{
        background-color: transparent !important;
    }

    .item:hover {
        cursor: pointer;
    }

    .done:hover {
        cursor: pointer;
    }

    .done {
        text-shadow: 1px 1px #252525;
        border: none;
        font-weight: 250;
        color: rgb(238, 238, 238);
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
<li class="todo-item"><button class="done">{description}</button></li>
{:else}
<li class="todo-item"><button class="item">{description}</button></li>
{/if}

