<script lang="ts">
	import { page } from "$app/stores";
	import { slide } from "svelte/transition";
	import { dictionary } from "../stores";
	import Base from "./base.svelte";
	import { sleep } from "../functions";
	import { goto } from "$app/navigation";

    export let active : boolean = false;

    let firstFocusableElement: HTMLAnchorElement;
    let lastFocusableElement: HTMLAnchorElement;

    async function linkBehaviour(href: string) {
        active = false

        await sleep(2)
        goto(href)
    }
</script>

<Base bind:active firstFocusableElement={firstFocusableElement} lastFocusableElement={lastFocusableElement}>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div on:click|stopPropagation transition:slide|global={{duration:200, axis: 'x'}}>
        <nav>
            <img class="logo" src="/images/ccs/CCSlogoTransparent.webp" alt="CCS Logo">
            
            <a href="/" on:click|preventDefault={() => linkBehaviour('/')} class="baseButton {$page.url.pathname === '/' ? 'active' : ''}" bind:this={firstFocusableElement}>{$dictionary.inicio}</a>
            <a href="/acerca" on:click|preventDefault={() => linkBehaviour('/acerca')} class="baseButton {$page.url.pathname === '/acerca' ? 'active' : ''}">{$dictionary.acercaDeNosotros}</a>
            <a href="/contactanos" on:click|preventDefault={() => linkBehaviour('/contactanos')} class="baseButton {$page.url.pathname === '/contactanos' ? 'active' : ''}">{$dictionary.contactanos}</a>
        </nav>
        <section>
            <a class="baseButton" href="https://www.instagram.com/ccsguitars" target="_blank" aria-label="Instagram" bind:this={lastFocusableElement}>
                <ion-icon name="logo-instagram"></ion-icon>
                <span>Instagram</span>
            </a>
        </section>
    </div>
</Base>

<style>
    div {
        position: fixed;
        top: 0;
        left: 0;
        height: 100vh;
        height: 100dvh;
        overflow-y: auto;
        z-index: 3;
        background-color: var(--main);
        border-right: solid 2px var(--content);
        padding: 2em;

        display: flex;
        flex-direction: column;
        justify-content: space-between;
        gap: 4rem;
    }
    
    nav {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: flex-end;
        gap: 1em;
    }

    section {
        display: flex;
        justify-content: center;
        margin-bottom: 1rem;
    }

    section .baseButton {
        font-size: 1.2rem;
    }

    nav .baseButton {
        font-size: 1.1rem;
        min-width: 200px;
        width: 100%;
        justify-content: flex-start;
    }

    .logo {
        height: 40px;
        width: auto;
        
        margin: 40px auto;
        -webkit-user-drag: none;
    }

    @media screen and (max-width: 500px){
        div {
            padding: 1em;
        }
    }
</style>