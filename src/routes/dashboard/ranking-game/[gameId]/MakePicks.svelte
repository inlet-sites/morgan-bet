<script>
    import {getContext} from "svelte";

    const loader = getContext("loader");
    const notify = getContext("notify");
    let {teams, gameId, addPicks} = $props()
    let confirmModal = $state(false);

    const move = (i, direction)=>{
        if(i - direction < 0 || i - direction > teams.length-1) return;
        const temp = teams[i-direction];
        teams[i-direction] = teams[i];
        teams[i] = temp;
    }

    const submit = ()=>{
        loader(true);
        const teamList = [];
        for(let i = 0; i < teams.length; i++){
            teamList.push(teams[i]._id);
        }
        fetch(`${import.meta.env.VITE_APIURL}/rankinggame/${gameId}/picks`, {
            method: "put",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${localStorage.getItem("userToken")}`
            },
            body: JSON.stringify({picks: teamList})
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    addPicks(teamList);
                }
            })
            .catch((err)=>{
                console.log(err);
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader(false);
            });
    }
</script>

<div class="MakePicks">
    {#if confirmModal}
        <div class="confirmModal">
            <div class="confirm">
                <h1>Once set, your picks cannot be changed</h1>
                <p>Are you sure you are happy with your picks?</p>

                <div class="confirmButtons">
                    <button class="button no" onclick={()=>{confirmModal = false}}>No</button>
                    <button class="button" onclick={submit}>Yes</button>
                </div>
            </div>
        </div>
    {/if}

    <h1>Make your picks:</h1>

    <div class="teams">
        {#each teams as team, i}
            <div class="team">
                <img src="/mlbLogos/{team.name.replaceAll(" ", "")}.png" alt="Team logo">

                <p>{team.location} {team.name}</p>

                <p class="points"><span class="scarlet">{i+1}</span>: {teams.length-i} pts/game</p>

                <div class="teamButtons">
                    <button onclick={()=>{move(i, 1)}}>
                        <svg width="24px" height="24px" stroke-width="2.5" viewBox="0 0 24 24" fill="none" color="currentColor">
                            <path d="M6 15L12 9L18 15" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"></path>
                        </svg>
                    </button>

                    <button onclick={()=>{move(i, -1)}}>
                        <svg width="24px" height="24px" stroke-width="1.5" viewBox="0 0 24 24" fill="none" color="currentColor">
                            <path d="M6 9L12 15L18 9" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"></path>
                        </svg>
                    </button>
                </div>
            </div>
        {/each}
    </div>

    <button class="button" onclick={()=>{confirmModal = true}}>Confirm Picks</button>
</div>

<style>
    .MakePicks{
        position: relative;
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
    }

    .team img{
        height: 100%;
        margin-right: 35px;
    }

    .team p{
        font-size: 22px;
    }

    .points{
        margin-left: auto;
    }

    .teamButtons{
        display: flex;
        flex-direction: column;
        margin: 0 15px 0 15px;
    }

    .teamButtons button{
        background: none;
        border: none;
        cursor: pointer;
        color: var(--scarlet);
    }

    .teamButtons button:hover{
        color: var(--claret);
    }

    .teamButtons svg{
        height: 35px;
        width: 35px;
    }

    .scarlet{
        color: var(--scarlet);
    }

    .confirmModal{
        display: flex;
        justify-content: center;
        align-items: center;
        position: fixed;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        z-index: 50;
        backdrop-filter: blur(10px);
    }

    .confirm{
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        background: var(--prussianBlue);
        color: var(--platinum);
        width: 100%;
        max-width: 500px;
    }

    .confirm > *{
        margin: 25px;
    }

    .confirmButtons{
        display: flex;
        justify-content: space-around;
        width: 100%;
    }

    .no{
        background: var(--scarlet);
    }
</style>
