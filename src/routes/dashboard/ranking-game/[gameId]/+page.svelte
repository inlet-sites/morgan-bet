<script>
    import {onMount, getContext, tick} from "svelte";
    import MakePicks from "./MakePicks.svelte";
    import ViewRequests from "./ViewRequests.svelte";
    import Rankings from "./Rankings.svelte";
    import Players from "./Players.svelte";

    const loader = getContext("loader");
    const notify = getContext("notify");
    const {data}  = $props();
    let game = $state();
    let teams = $state();
    let ready = $state(false);
    let viewRequests = $state(false);
    let viewRankings = $state(false);
    let makePicks = $state(false);

    const userAccepted = (user)=>{
        game.players.push({
            user: user._id,
            name: user.name,
            picks: []
        });
    }

    onMount(()=>{
        loader(true);
        const userToken = localStorage.getItem("userToken");
        fetch(`${import.meta.env.VITE_APIURL}/rankinggame/${data.gameId}`, {
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
    {#if ready}
        {#if makePicks}
            <MakePicks
                teams={teams}
                gameId={game._id}
                addPicks={(p)=>{player.picks = p}}
            />
        {:else if viewRequests}
            <ViewRequests
                requests={game.joinRequests}
                gameId={game.id}
                accept={userAccepted}
                close={()=>{viewRequests = false}}
            />
        {:else if viewRankings}
            <Rankings
                players={game.players}
                teams={teams}
                close={()=>{viewRankings = false}}
            />
        {:else}
            <Players
                game={game}
                teams={teams}
                viewRequests={()=>{viewRequests = true}}
                viewRankings={()=>{viewRankings = true}}
            />
        {/if}
    {/if}
</div>

<style>
    .RankingGame{
        padding: 35px;
        position: relative;
    }
</style>
