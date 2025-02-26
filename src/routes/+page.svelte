<script>
    import "$lib/global.css";
</script>

<div class="container">
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
