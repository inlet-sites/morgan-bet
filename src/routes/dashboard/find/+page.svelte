<script>
    import {onMount, getContext} from "svelte";
    import {goto} from "$app/navigation";
    import gameLogo from "$lib/images/rankingGame.webp";

    const loader = getContext("loader");
    const notify = getContext("notify");
    let games = $state();
    let joinModal = $state(null);
    let apiUrl = import.meta.env.VITE_APIURL;
    $effect(()=>{
        if(joinModal){
            document.body.overflow = "hidden";
        }else{
            document.body.overflow = "auto";
        }
    });

    const joinGame = ()=>{
        loader(true);
        fetch(`${apiUrl}/rankinggame/${joinModal.id}/join`, {
            method: "put",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${localStorage.getItem("userToken")}`
            }
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    notify("success", "Request sent. Awaiting approval");
                    goto("/dashboard");
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

    onMount(()=>{
        loader(true);
        fetch(`${apiUrl}/rankinggame/available`, {
            method: "get",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${localStorage.getItem("userToken")}`
            }
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    games = response;
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

<svelte:head>
    <title>Find Games | MorganBet</title>
</svelte:head>

<div class="FindGames">
    {#if joinModal}
        <div class="joinModal">
            <div class="join">
                <h4>Would you like to request to join this game?</h4>
                <h2>{joinModal.name}</h2>
                <h3>Ranking Game</h3>
                <div class="buttonBox">
                    <button class="button" onclick={joinGame}>Join</button>
                    <button class="button cancel" onclick={()=>{joinModal = null}}>Cancel</button>
                </div>
            </div>
        </div>
    {/if}
    <h1>Available Games</h1>

    {#each games as game}
        <button class="optionButton" onclick={()=>{joinModal = game}}>
            <img src={gameLogo} alt="Ranking Game">
            <h2>{game.name}</h2>
        </button>
    {/each}
</div>

<style>
    .FindGames{
        padding: 35px;
    }

    .joinModal{
        display: flex;
        justify-content: center;
        align-items: center;
        position: fixed;
        top: 0;
        left: 0;
        height: 100vh;
        width: 100vw;
        backdrop-filter: blur(10px);
    }

    .join{
        display: flex;
        flex-direction: column;
        align-items: center;
        background: var(--prussianBlue);
        color: var(--platinum);
        width: 100%;
        max-width: 500px;
        padding: 35px;
    }

    .buttonBox{
        display: flex;
        justify-content: space-around;
        margin-top: 35px;
        width: 100%;
    }

    .buttonBox button{
        width: 150px;
    }

    .cancel{
        background: var(--scarlet);
    }

    h1{
        margin-bottom: 35px;
    }

    @media screen and (max-width: 500px){
        .FindGames{
            padding: 15px;
        }
    }
</style>
