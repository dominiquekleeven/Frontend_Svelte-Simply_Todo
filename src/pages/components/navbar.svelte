

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
    <a href="/" class="nav-item"><Icon icon={home_icon} /></a>


    <span class="acc-buttons">
        {#if authorized()}
        <button on:click={logout} class="nav-item">
            <Icon icon={logout_icon}></Icon>
        </button>
        {:else}
        <a href="/login" class="nav-item">
            <Icon icon={login_icon}></Icon>
         </a>
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
    }
    .nav-item:hover{
        cursor: pointer;
        color: rgb(155, 37, 102);
    }
    
    button{
        margin: 0;
        padding: 0;
        background-color: transparent;
        border: none;
    }
    </style>