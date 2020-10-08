<script>
    import Todolist from "./components/todolist.svelte";
    import AudioPlayer from "./components/audioplayer.svelte"
    import Header from "./components/header.svelte";
</script>

<style>
    main {
        display: flex;
        flex-direction: column !important;
        align-items: center;
    }
</style>

<main>
    <Header />

    <!-- <AudioPlayer
	src="https://sveltejs.github.io/assets/music/satie.mp3"
	title="Gymnopédie no. 1"
	composer="Erik Satie"
	performer="Prodigal Procrastinator"
/> -->

    <Todolist></Todolist>
</main>
