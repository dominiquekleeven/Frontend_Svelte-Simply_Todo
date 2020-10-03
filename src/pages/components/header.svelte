<script>
    import { onMount } from "svelte";
    let time = new Date().toLocaleString('en-NL');

    onMount(async () => {
        timer();
    })

    function timer()
    {
        setInterval(() => {
            time = new Date().toLocaleString('en-NL')
        }, 1000)
    }
</script>

<style>
    .header {
        text-align: center;
        padding: 1em;
        margin: 0 auto;
    }

    @media (max-width: 700px) {
        h1 {
            margin-top: 70px;
            font-size: 3em !important;
        }
    }

    h1 {
        color: rgb(155, 37, 102);
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 300;
        margin-bottom: 0;
    }

    .subtitle {
        color: rgb(56, 56, 56);
        font-size: 1.5em;
        font-style: italic;
        margin-top: 0;
    }

    .time {
        margin-top: -10px;
    }
</style>

<div class="header">
    <h1>Simply Todo</h1>
    <p class="subtitle">Manage your day</p>
    <p class="time">{time}</p>
</div>
