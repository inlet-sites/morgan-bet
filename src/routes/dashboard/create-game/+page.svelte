<script>
    import rankingGameImg from "$lib/images/rankingGame.webp";
</script>

<div class="CreateGame">
    <h1>Create New Game</h1>

    <a href="/dashboard/create-game/ranking-game" class="game">
        <img src={rankingGameImg} alt="Ranking Game">
        <h2>MLB Ranking Game</h2>
    </a>
</div>

<style>
    .CreateGame{
        padding: 35px;
    }

    h1{
        margin-bottom: 35px;
    }

    .game{
        display: flex;
        align-items: center;
        height: 75px;
        width: 100%;
        max-width: 750px;
        background: var(--prussianBlue);
        color: var(--platinum);
        border: 2px solid var(--prussianBlue);
        text-decoration: none;
    }

    .game:hover{
        border: 2px solid var(--scarlet);
    }

    .game img{
        height: 100%;
        margin-right: 35px;
    }

    @media screen and (max-width: 500px){
        .CreateGame{
            padding: 15px;
        }
    }
</style>
