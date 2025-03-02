<script>
    import {onMount, getContext, tick} from "svelte";
    import MakePicks from "./MakePicks.svelte";

    const loader = getContext("loader");
    const notify = getContext("notify");
    const user = getContext("user");
    const {data}  = $props();
    let game = $state();
    let teams = $state();
    let player = $state();
    let ready = $state(false);
    let select = $state();
    const apiUrl = import.meta.env.VITE_APIURL;

    const addPicks = (picks)=>{
        player.picks = picks;
    }

    const calculateTotal = ()=>{
        let total = 0;
        for(let i = 0; i < player.picks.length; i++){
            const team = teams.find(t => t.id === player.picks[i]);
            total += (player.picks.length - i) * team.wins;
        }
        return total;
    }

    const updatePlayer = ()=>{
        player = game.players.find(p => p.user === select.value);
    }

    onMount(()=>{
        loader(true);
        const userToken = localStorage.getItem("userToken");
        fetch(`${apiUrl}/rankinggame/${data.gameId}`, {
            method: "get",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${userToken}`
            }
        })
            .then(r=>r.json())
            .then(async (response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    game = response.game;
                    teams = response.teams;
                    player = game.players.find(p => p.user === $user.id);
                    ready = true;
                }
            })
            .catch((err)=>{
                console.log(err);
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader(false);
            });
    });
</script>

<div class="RankingGame">
    {#if game?.players.find(p => p.user === $user.id).picks.length === 0}
        <MakePicks
            teams={teams}
            gameId={game._id}
            addPicks={(p)=>{player.picks = p}}
        />
    {:else if ready}
        <h1>{game.name}</h1>

        <select value={player?.user} onchange={updatePlayer} bind:this={select}>
            {#each game.players as p}
                <option value={p.user}>{p.name}</option>
            {/each}
        </select>

        {#if player.picks.length === 0}
            <h1 class="nopicks">{player.name} has not yet made his/her picks</h1>
        {:else}
            <p class="total">Total: <span class="scarlet">{calculateTotal()}</span></p>
            <div class="teams">
                {#each player.picks as p, i}
                    {@const team = teams.find(t => t.id === p)}
                    <div class="team">
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
    .RankingGame{
        padding: 35px;
    }

    .team{
        display: flex;
        align-items: center;
        height: 75px;
        width: 100%;
        max-width: 750px;
        background: var(--prussianBlue);
        color: var(--platinum);
        margin: 15px 0;
        font-size: 22px;
        padding-right: 15px;
    }

    .team img{
        height: 100%;
        margin-right: 35px;
    }

    .calcs{
        margin-left: auto;
    }

    .scarlet{
        color: var(--scarlet);
    }

    .total{
        width: 100%;
        max-width: 750px;
        text-align: right;
        font-size: 22px;
        padding-right: 15px;
    }

    select{
        font-size: 22px;
        padding: 5px 15px;
        border: 1px solid var(--scarlet);
    }

    .nopicks{
        font-size: 35px;
        margin-top: 35px;
    }

    @media screen and (max-width: 750px){
        .team{
            flex-direction: column;
            align-items: flex-start;
            height: initial;
            padding: 15px;
        }

        .team img{
            width: 75px;
        }

        .calcs{
            margin-left: 0;
            font-size: 18px;
        }
    }
</style>
