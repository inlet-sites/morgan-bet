<script>
    import logo from "$lib/images/logo.svg";
    import {afterNavigate} from "$app/navigation";

    let options = $state();
    let menuOpen = $state(false);

    afterNavigate(()=>{
        if(menuOpen) toggleMenu();
    });

    const toggleMenu = ()=>{
        if(menuOpen){
            menuOpen = false;
            document.body.overflow = "auto";
            options.style.display = "none";
        }else{
            menuOpen = true;
            document.body.overflow = "hidden";
            options.style.display = "flex";
        }
    }

    const logout = ()=>{
        localStorage.removeItem("userToken");
        window.location.href = "/";
    }
</script>

<header>
    <a class="logo" href="/">
        <img src={logo} alt="Logo">
        <p>MorganBet</p>
    </a>
    
    <div class="options" bind:this={options}>
        <a class="optionItem" href="/dashboard">My Games</a>
        <div class="divider"></div>
        <a class="optionItem" href="/dashboard/find">Find Games</a>
        <div class="divider"></div>
        <button class="optionItem" onclick={logout}>Logout</button>
    </div>

    <button class="hamburger" aria-label="menu" onclick={toggleMenu}>
        <svg width="35px" height="35px" stroke-width="1.5" viewBox="0 0 24 24" fill="none" color="currentColor">
            <path d="M3 5H21" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"></path>
            <path d="M3 12H21" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"></path>
            <path d="M3 19H21" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"></path>
        </svg>
    </button>
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

    .optionItem{
        color: var(--platinum);
        font-size: 20px;
        text-decoration: none;
        background: none;
        border: none;
    }

    .optionItem:hover{
        color: var(--scarlet);
        cursor: pointer;
    }

    .divider{
        border-right: 2px solid var(--claret);
        margin: 0 15px;
    }

    .hamburger{
        display: none;
        color: var(--scarlet);
        background: none;
        border: none;
        margin-right: 35px;
        cursor: pointer;
        margin-left: auto;
    }

    @media screen and (max-width: 800px){
        .options{
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: fixed;
            top: 75px;
            left: 0;
            height: 100vh;
            width: 100vw;
            background: var(--prussianBlue);
            margin: 0;
            padding: 0 0 150px 0;
        }

        .options a{
            margin: 35px;
            font-size: 35px;
        }

        .divider{
            display: none;
        }

        .hamburger{
            display: flex;
        }
    }
</style>
