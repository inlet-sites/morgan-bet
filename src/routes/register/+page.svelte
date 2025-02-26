<script>
    import {onMount} from "svelte";
    import "$lib/global.css";
    import Notifier from "$lib/Notifier.svelte";

    let name = $state();
    let email = $state();
    let password = $state();
    let confirmPassword = $state();
    let initialFocus = $state();
    let loader = $state(false);

    const submit = ()=>{
        loader = true;
        setTimeout(()=>{loader = false}, 2000);
    }

    onMount(()=>{
        initialFocus.focus();
    });
</script>

{#if loader}
    <Notifier/>
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
