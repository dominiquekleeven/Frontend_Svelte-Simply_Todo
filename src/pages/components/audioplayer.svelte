<script context="module">
    const elements = new Set();
</script>

<script>
    import { onMount } from "svelte";

    export let src;
    export let title;
    export let composer;
    export let performer;

    let audio;
    let paused = true;

    onMount(() => {
        elements.add(audio);
        return () => elements.delete(audio);
    });
</script>

<style>
    article {
        margin: 0 0 1em 0;
        max-width: 800px;
    }
    h2,
    p {
        margin: 0 0 0.3em 0;
    }

    h2{
        font-weight: 300;
    }

    p{
        color: rgb(44, 44, 44);
    }
    audio {
        width: 100%;
    }
    audio:focus{
        outline: none;
        border-radius: 0.25em;
    }


    .playing {
        color: rgb(155, 37, 102);
    }
</style>

<article class:playing={!paused}>
    <h2>{title}</h2>
    <p><strong>{composer}</strong> / performed by {performer}</p>
    <!-- svelte-ignore a11y-media-has-caption -->
    <audio bind:this={audio} bind:paused controls {src} />
</article>
