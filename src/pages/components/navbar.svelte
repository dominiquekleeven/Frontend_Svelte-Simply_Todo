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
</style>

<script>
    import Icon from 'fa-svelte';
    import { faList } from '@fortawesome/free-solid-svg-icons/faList'
    import { faSignInAlt } from '@fortawesome/free-solid-svg-icons/faSignInAlt'
    let list_icon = faList;
    let login_icon = faSignInAlt;
</script>

<nav class="navbar">
    <a href="/" class="nav-item"><Icon icon={list_icon} /></a>


    <a href="/login" class="nav-item">
       <Icon icon={login_icon}></Icon>
    </a>
</nav>