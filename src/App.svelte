<script>
	import Main from './pages/main.svelte';
	import Signup from './pages/signup.svelte'
	import Login from './pages/login.svelte'
	import Router from 'page';
	import Navbar from './pages/components/navbar.svelte'

	
	let page;

	Router('/', () => page = Main);
	Router('/signup', () => page = Signup)
	Router('/login', () => page = Login)
	Router.start();

</script>

<Navbar></Navbar>


<svelte:component this={page} />