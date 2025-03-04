<script>
    import "$lib/global.css";
    import {goto} from "$app/navigation";
    import logo from "$lib/images/logo.svg";
    import LogoHeader from "$lib/LogoHeader.svelte";

    const dashboard = ()=>{
        if(localStorage.getItem("userToken")){
            goto("/dashboard");
        }else{
            goto("/signin");
        }
    }
</script>

<svelte:head>
    <title>404 Error | MorganBet</title>
</svelte:head>

<div class="container">
    <LogoHeader/>

    <img src={logo} alt="MorganBet logo">
    <h1>Morgan Bet</h1>

    <h2>The page that you are looking for does not exist</h2>

    <div class="buttonBox">
        <a class="button" href="/">Home</a>
        <button class="button" onclick={dashboard}>Dashboard</button>
    </div>
</div>

<style>
    .container{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh;
        width: 100vw;
    }

    img{
        max-height: 50%;
    }

    .buttonBox{
        margin-top: 35px;
    }
</style>
