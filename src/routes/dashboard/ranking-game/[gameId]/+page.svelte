<script>
    import {onMount, getContext} from "svelte";
    import MakePicks from "./MakePicks.svelte";

    const loader = getContext("loader");
    const notify = getContext("notify");
    const user = getContext("user");
    const {data}  = $props();
    let game = $state();
    let teams = $state();
    let player = $state();
    let ready = $state(false);
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
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    console.log(response);
                    game = response.game;
                    teams = response.teams;
                    player = game.players.find(p => p.user === $user.id);
                    ready = true;
                }
            })
            .catch((err)=>{
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader(false);
            });
    });
</script>

<div class="RankingGame">
    {#if player?.picks.length === 0}
        <MakePicks
            teams={teams}
            gameId={game._id}
            addPicks={(p)=>{player.picks = p}}
        />
    {:else if ready}
        <h1>{game.name}</h1>

        <select>
            {#each game.players as p}
                <option onchange={()=>{player = p}}>{p.name}</option>
            {/each}
        </select>

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
</style>
