<script>
    import {getContext} from "svelte";

    const user = getContext("user");
    let {game, teams, viewRequests, viewRankings} = $props();
    let playerIndex = $state(0)
    let player = $state(game.players.find(p => p.user = $user.id));
    let owner = $state(game.owner === $user.id);

    const updatePlayer = ()=>{
        player = game.players[playerIndex];
    }

    const getTotal = ()=>{
        let total = 0;
        for(let i = 0; i < player.picks.length; i++){
            const team = teams.find(t => t.id === player.picks[i]);
            total += (player.picks.length - i) * team.wins;
        }
        return total;
    }
</script>

<div class="Players">
    <div class="gameButtons">
        {#if owner}
            <button class="button" onclick={viewRequests}>Join Requests</button>
        {/if}
        <button class="button" onclick={viewRankings}>Rankings</button>
    </div>

    <h1>{game.name}</h1>

    {#if game.players.length === 0}
        <h2>No players yet</h2>
    {:else}
        <select bind:value={playerIndex} onchange={updatePlayer}>
            {#each game.players as player, i}
                <option value={i}>{player.name}</option>
            {/each}
        </select>

        {#if player.picks.length === 0}
            <h3>{player.name} has not yet made his/her picks</h3>
        {:else}
            <p class="total">Total: <span class="scarlet">{getTotal()}</span></p>
            <div class="picks">
                {#each player.picks as pick, i}
                    {@const team = teams.find(t => t.id === pick)}
                    <div class="pick">
                        <img src="/mlbLogos/{team.name.replaceAll(" ", "")}.png" alt="{team.name} logo">
                        <p>{team.name}</p>
                        <p class="calcs">{team.wins} wins x {teams.length - i} pts/game = <span class="scarlet">{team.wins * (teams.length - i)}</span></p>
                    </div>
                {/each}
            </div>
        {/if}
    {/if}
</div>

<style>
    select{
        font-size: 22px;
        padding: 5px 15px;
        border: 1px solid var(--scarlet);
    }

    .pick{
        display: flex;
        align-items: center;
        height: 75px;
        width: 100%;
        max-width: 750px;
        background: var(--prussianBlue);
        color: var(--platinum);
        margin: 15px 0;
        padding-right: 15px;
        font-size: 22px;
    }

    .pick img{
        height: 100%;
        margin-right: 35px;
    }

    .calcs{
        font-size: 18px;
        margin-left: auto;
    }

    .total{
        display: flex;
        justify-content: flex-end;
        padding-right: 15px;
        width: 100%;
        max-width: 750px;
        font-size: 22px;
    }

    .scarlet{
        color: var(--scarlet);
    }

    .gameButtons{
        display: flex;
        flex-direction: column;
        position: absolute;
        top: 35px;
        right: 35px;
    }

    .gameButtons > *{
        margin: 10px 0;
    }
</style>
