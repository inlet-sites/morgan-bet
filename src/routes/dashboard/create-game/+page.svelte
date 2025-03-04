<script>
    import rankingGameImg from "$lib/images/rankingGame.webp";
</script>

<div class="CreateGame">
    <h1>Create New Game</h1>

    <a class="optionButton" href="/dashboard/create-game/ranking-game">
        <img src={rankingGameImg} alt="Ranking Game">
        <h2>MLB Ranking Game</h2>
    </a>
</div>

<svelte:head>
    <title>Create Game | MorganBet</title>
</svelte:head>

<style>
    .CreateGame{
        padding: 35px;
    }

    h1{
        margin-bottom: 35px;
    }

    @media screen and (max-width: 500px){
        .CreateGame{
            padding: 15px;
        }
    }
</style>
