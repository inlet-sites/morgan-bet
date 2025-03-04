<script>
    import {getContext, tick} from "svelte";
    import americanLeagueImg from "$lib/images/americanLeague.png";
    import nationalLeagueImg from "$lib/images/nationalLeague.png";
    import mlbLogo from "$lib/images/mlbLogo.png";

    const loader = getContext("loader");
    const notify = getContext("notify");
    let stage = $state("league");
    let league = $state();
    let season = $state(new Date().getFullYear());
    let part = $state();
    let name = $state();
    let nameInput = $state();
    let joinBy = $state();

    const chooseLeague = (l)=>{
        league = l;
        stage = "season";
    }

    const chooseSeason = async (p)=>{
        part = p;
        stage = "name";
        await tick();
        nameInput.focus();
    }

    const submit = ()=>{
        const date = new Date(joinBy);
        date.setDate(date.getDate() + 1);

        loader(true);
        fetch(`${import.meta.env.VITE_APIURL}/rankinggame`, {
            method: "post",
            headers: {
                "Content-type": "application/json",
                Authorization: `Bearer ${localStorage.getItem("userToken")}`
            },
            body: JSON.stringify({
                name: name,
                season: season,
                part: part,
                league: league,
                joinByDate: date
            })
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    window.location.href = `/dashboard/ranking-game/${response.id}`;
                }
            })
            .catch((err)=>{
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader(false);
            });
    }
</script>

<svelte:head>
    <title>Create Ranking Game | MorganBet</title>
</svelte:head>

<div class="CreateRankingGame">
    <h1>Create New Ranking Game</h1>

    {#if stage === "league"}
        <h2>Choose League:</h2>

        <button class="optionButton" onclick={()=>{chooseLeague("american")}}>
            <img src={americanLeagueImg} alt="American League logo">
            <h2>American League</h2>
        </button>

        <button class="optionButton" onclick={()=>{chooseLeague("national")}}>
            <img src={nationalLeagueImg} alt="National League logo">
            <h2>National League</h2>
        </button>

        <button class="optionButton" onclick={()=>{chooseLeague("all")}}>
            <img class="mlbLogo" src={mlbLogo} alt="MLB logo">
            <h2>All Teams</h2>
        </button>
    {:else if stage === "season"}
        <h2>Choose Season ({season})</h2>

        <button class="optionButton" onclick={()=>{chooseSeason("full")}}>
            <img src={mlbLogo} alt="Entire season">
            <h2>Full Season</h2>
        </button>

        <button class="optionButton" onclick={()=>{chooseSeason("firstHalf")}}>
            <img src={americanLeagueImg} alt="First Half of season">
            <h2>First Half of Season</h2>
        </button>

        <button class="optionButton" onclick={()=>{chooseSeason("secondHalf")}}>
            <img src={nationalLeagueImg} alt="Second Half of season">
            <h2>Second Half of Season</h2>
        </button>
    {:else if stage === "name"}
        <form onsubmit={submit}>
            <label>Game Title:
                <input
                    type="text"
                    bind:value={name}
                    bind:this={nameInput}
                    placeholder="Title"
                    required
                >
            </label>

            <label>Last Date to Join:
                <input
                    type="date"
                    bind:value={joinBy}
                    required
                >
            </label>

            <button class="button">Create</button>
        </form>
    {/if}
</div>

<style>
    .CreateRankingGame{
        padding: 35px;
    }

    h1{
        margin-bottom: 35px;
    }

    .mlbLogo{
        background: white;
    }

    form{
        display: flex;
        flex-direction: column;
        width: 100%;
        max-width: 750px;
    }

    form label{
        display: flex;
        flex-direction: column;
        font-size: 22px;
    }

    form input{
        margin-bottom: 15px;
        font-size: 22px;
        padding: 5px 0 5px 10px;
        background: var(--platinum);
        border: 3px solid var(--prussianBlue);
        color: var(--prussianBlue);
        outline: none;
    }

    form input:focus{
        border: 4px solid var(--scarlet);
    }

    @media screen and (max-width: 500px){
        .CreateRankingGame{
            padding: 15px;
        }
    }
</style>
