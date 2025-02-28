<script>
    let {game} = $props();
    let link = $state(`/dashboard/rankinggame/${game.id}`);
</script>

<a href={link} class="Game">
    <h2>{game.name}</h2>

    <p>Ranking Game</p>
</a>

<style>
    .Game{
        display: flex;
        align-items: center;
        background: var(--prussianBlue);
        color: var(--platinum);
        height: 75px;
        margin: 35px 0;
        padding: 15px;
        width: 100%;
        max-width: 750px;
        text-decoration: none;
    }

    .Game:hover{
        color: var(--scarlet);
    }

    p{
        margin-left: auto;
    }
</style>
