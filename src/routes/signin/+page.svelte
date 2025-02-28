<script>
    import {onMount} from "svelte";
    import "$lib/global.css";
    import Loader from "$lib/Loader.svelte";
    import Notifier from "$lib/Notifier.svelte";

    let email = $state();
    let password = $state();
    let loader = $state(false);
    let notifier = $state(null);
    let initialFocus = $state();

    const notify = (type, message)=>{
        notifier = {
            type: type,
            message: message
        };

        setTimeout(()=>{
            notifier = null;
        }, 7500);
    }

    const submit = ()=>{
        loader = true;
        fetch(`${import.meta.env.VITE_APIURL}/user/token`, {
            method: "post",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                email: email,
                password: password
            })
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    localStorage.setItem("userToken", response.token);
                    window.location.href = "/dashboard";
                }
            })
            .catch((err)=>{
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader = false;
            });
    }

    onMount(()=>{
        initialFocus.focus();
    });
</script>

{#if loader}
    <Loader/>
{/if}

{#if notifier}
    <Notifier type={notifer.type} message={notifier.message}/>
{/if}

<div class="container">
    <form class="standardForm" onsubmit={submit}>
        <h1>Sign In</h1>

        <label>Email
            <input
                type="email"
                bind:value={email}
                bind:this={initialFocus}
                placeholder="Email"
                required
            >
        </label>

        <label>Password
            <input
                type="password"
                bind:value={password}
                placeholder="Password"
                required
            >
        </label>

        <button class="button">Sign In</button>
    </form>
</div>

<style>
    .container{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        width: 100vw;
    }
</style>
