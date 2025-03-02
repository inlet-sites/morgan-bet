<script>
    let {players, teams, close} = $props();
    let rankings = $state();

    const score = (player)=>{
        let total = 0;
        for(let i = 0; i < player.picks.length; i++){
            const team = teams.find(t => t.id === player.picks[i]);
            total += (player.picks.length -i) * team.wins;
        }
        return total;
    }

    for(let i = 0; i < players.length; i++){
        players[i].score = score(players[i]);
    }

    players.sort((a, b) => a.score < b.score);
</script>

<div class="Rankings">
    <h1>Rankings</h1>

    {#each players as player}
        <div class="player">
            <p>{player.name}</p>
            {#if player.picks.length === 0}
                <p>No Picks</p>
            {:else}
                <p class="scarlet">{score(player)}</p>
            {/if}
        </div>
    {/each}

    <div class="button close" onclick={close}>Back</div>
</div>

<style>
    .player{
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 75px;
        width: 100%;
        max-width: 750px;
        background: var(--prussianBlue);
        color: var(--platinum);
        font-size: 22px;
        padding: 0 15px;
        margin: 15px 0;
    }

    .scarlet{
        color: var(--scarlet);
    }

    .close{
        position: absolute;
        top: 35px;
        right: 35px;
    }
</style>
