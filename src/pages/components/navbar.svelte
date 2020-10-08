

<script>
    import Icon from 'fa-svelte';
    import { faHome } from '@fortawesome/free-solid-svg-icons/faHome'
    import { faSignInAlt } from '@fortawesome/free-solid-svg-icons/faSignInAlt'
    import { faSignOutAlt } from '@fortawesome/free-solid-svg-icons/faSignOutAlt'
    let home_icon = faHome;
    let login_icon = faSignInAlt;
    let logout_icon = faSignOutAlt;

    function authorized()
    {
        return localStorage.getItem("token") != undefined
    }

    function logout()
    {
        localStorage.clear();
        document.location.href = "/login"
    }


</script>

<nav class="navbar">
    <button href="/" class="nav-item"><Icon icon={home_icon} /></button>


    <span class="acc-buttons">
        {#if authorized()}
        <button on:click={logout} class="nav-item">
            <Icon icon={logout_icon}></Icon>
        </button>
        {:else}
        <button href="/login" class="nav-item">
            <Icon icon={login_icon}></Icon>
         </button>
        {/if}
    </span>
</nav>


<style>
    .navbar {
        height: 60px;
        width: 100%;
        position:fixed;
        display: flex;
        flex-wrap: nowrap;
        justify-content: space-between;
        align-items: baseline;
        padding: 25px;
        box-sizing: border-box;
    }
    
    .nav-item{
        font-size: 25px;
        color: white;
        border-radius: 0.5em;
        background-color: rgba(0, 0, 0, 0.719);
        padding: 10px;


    }
    .nav-item:hover{
        cursor: pointer;
        color: rgb(241, 174, 86);
    }
    
    button{
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0;
        padding: 0;
        height: 50px;
        width: 50px;
        background-color: transparent;
        border: none;
    }
    </style>