<script>
    import logo from "$lib/images/logo.svg";
</script>

<header>
    <a class="logo" href="/">
        <img src={logo} alt="Logo">
        <p>MorganBet</p>
    </a>
    
    <div class="options">
        <a href="/dashboard">My Games</a>
        <div class="divider"></div>
        <a href="/dashboard/find">Find Games</a>
    </div>
</header>

<style>
    header{
        display: flex;
        align-items: center;
        width: 100%;
        height: 75px;
        background: var(--prussianBlue);
    }

    .logo{
        display: flex;
        align-items: center;
        color: var(--platinum);
        height: 100%;
        font-size: 22px;
        text-decoration: none;
    }

    .logo img{
        height: 100%;
        margin-right: 15px;
    }

    .options{
        display: flex;
        margin-left: auto;
        padding-right: 35px;
    }

    .options a{
        color: var(--platinum);
        font-size: 20px;
        text-decoration: none;
    }

    .divider{
        border-right: 2px solid var(--claret);
        margin: 0 15px;
    }
</style>
