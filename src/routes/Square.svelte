<script lang="ts">
	import { getTwemojiUrl } from "./utils";

    export let emoji: string;
    export let selected: boolean;
    export let found: boolean;
</script>

<div class="square" class:flipped={selected}>
    <button on:click />
    {#if !found}
        <img alt={emoji} src={getTwemojiUrl(emoji)} />
    {/if}
</div>

<style>
    .square {
        display: flex;
        justify-content: center;
        align-items: center;
        background: yellow;
    }

    .flipped button {
        background: red;
    }

    img {
        width: 6em;
        height: 6em;
        pointer-events: none;
    }

    button {
        position: absolute;
        width: 100%;
        height: 100%;
    }
</style>