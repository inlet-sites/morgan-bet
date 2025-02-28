<script>
    import logo from "$lib/images/logo.svg";
</script>

<a href="/" class="header">
    <img src={logo} alt="MorganBet Logo">
    <p>Morgan Bet</p>
</a>

<style>
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
</style>
