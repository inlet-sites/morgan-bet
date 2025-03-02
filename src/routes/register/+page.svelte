<script>
    import {onMount} from "svelte";
    import "$lib/global.css";
    import Loader from "$lib/Loader.svelte";
    import Notifier from "$lib/Notifier.svelte";

    let name = $state();
    let email = $state();
    let password = $state();
    let confirmPassword = $state();
    let initialFocus = $state();
    let loader = $state(false);
    let notifier = $state(null);

    const submit = ()=>{
        if(password.length < 10){
            notify("error", "Password must contain at least 10 characters");
            return;
        }
        if(password !== confirmPassword){
            notify("error", "Passwords do not match");
            return;
        }

        loader = true;
        fetch(`${import.meta.env.VITE_APIURL}/user`, {
            method: "post",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                name: name,
                email: email,
                password: password,
                confirmPassword: confirmPassword
            })
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    window.location.href = "/signin";
                }
            })
            .catch((err)=>{
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader = false;
            });
    }

    const notify = (type, message)=>{
        console.log("notifying");
        notifier = {
            type: type,
            message: message
        }
        console.log(notifier);

        setTimeout(()=>{
            notifier = null;
        }, 7500);
    }

    onMount(()=>{
        initialFocus.focus();
    });
</script>

{#if loader}
    <Loader/>
{/if}

{#if notifier}
    <Notifier type={notifier.type} message={notifier.message}/>
{/if}

<div class="container">
    <form class="standardForm" onsubmit={submit}>
        <h1>Register</h1>

        <label>Full Name
            <input
                type="text"
                bind:value={name}
                bind:this={initialFocus}
                placeholder="Full name"
                required
            >
        </label>

        <label>Email
            <input
                type="email"
                bind:value={email}
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

        <label>Confirm Password
            <input
                type="password"
                bind:value={confirmPassword}
                placeholder="Password"
                required
            >
        </label>

        <button class="button">Sign Up</button>
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
