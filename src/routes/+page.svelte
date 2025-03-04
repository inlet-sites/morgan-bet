<script>
    import "$lib/global.css";
    import {goto} from "$app/navigation";
    import logo from "$lib/images/logo.svg";
    import LogoHeader from "$lib/LogoHeader.svelte";

    const signin = ()=>{
        if(localStorage.getItem("userToken")){
            goto("/dashboard");
        }else{
            goto("/signin");
        }
    }
</script>

<svelte:head>
    <title>MorganBet</title>
</svelte:head>

<div class="container">
    <LogoHeader/>

    <img class="mainLogo" src={logo} alt="MorganBet Logo">
    <h1>Morgan Bet</h1>

    <div class="buttons">
        <button class="button" onclick={signin}>Sign In</button>
        <a class="button" href="/register">Sign Up</a>
    </div>
</div>

<style>
    .container{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background: var(--platinum);
        height: 100vh;
        width: 100vw;
        position: relative;
    }

    .mainLogo{
        max-height: 25%;
    }

    h1{
        color: var(--prussianBlue);
        font-size: 65px;
        margin-bottom: 35px;
    }

    @media screen and (max-width: 500px){
        h1{
            font-size: 55px;
        }

        .button{
            display: flex;
            flex-direction: column;
            margin: 35px;
        }

        .buttons a{
            padding: 15px 45px;
        }
    }
</style>
