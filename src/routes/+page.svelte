<script>
    import "$lib/global.css";
    import logo from "$lib/images/logo.svg";
</script>

<div class="container">
    <a href="/" class="header">
        <img src={logo} alt="MorganBet Logo">
        <p>Morgan Bet</p>
    </a>

    <img class="mainLogo" src={logo} alt="MorganBet Logo">
    <h1>Morgan Bet</h1>

    <div class="buttons">
        <a class="button" href="/register">Sign Up</a>
        <a class="button" href="/signin">Sign In</a>
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

    .header{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 10%;
        position: absolute;
        top: 0;
        left: 0;
        padding: 15px;
        color: var(--prussianBlue);
        text-decoration: none;
    }

    .header img{
        height: 100%;
        margin-right: 15px;
    }

    .header p{
        font-size: 35px;
        font-weight: bold;
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
