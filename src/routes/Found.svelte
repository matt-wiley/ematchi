<script lang="ts">
	import { getTwemojiUrl } from "./utils";

    // Props
    export let found: string[];
</script>

<div class="found">
    {#each found as emoji}
        <img alt={emoji} src={getTwemojiUrl(emoji)} />
    {/each}
</div>

<style>
    .found {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 1em;
    }

    img { 
        width: 4em;
        height: 4em;
    }
</style>