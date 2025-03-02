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
    const apiUrl = import.meta.env.VITE_APIURL;

    const addPicks = (picks)=>{
        player.picks = picks;
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
                    game = response.game;
                    teams = response.teams;
                    player = game.players.find(p => p.user === $user.id);
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
    {:else}
        <h1>{game?.name}</h1>

        <div class="players">
            {#each game?.players as player}
                <button>{player.name}</button>
            {/each}
        </div>

        <div class="teams">
            {#each player?.picks as pick}
                <div class="team">

                </div>
            {/each}
        </div>
    {/if}
</div>

<style>
    .RankingGame{
        padding: 35px;
    }
</style>
