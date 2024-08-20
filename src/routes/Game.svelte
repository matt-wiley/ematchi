<script lang="ts">
	import Grid from "./Grid.svelte";
    import Found from "./Found.svelte";
    import { levels } from "./levels";
    import type { Level } from "./levels";
	import { shuffle } from "./utils";
	import Countdown from "./Countdown.svelte";
	import { onMount } from "svelte";

    const level = levels[0];

    let size: number;
    let grid: string[] = createGrid(level);
    let found: string[] = [];
    let remaining: number = level.duration;
    let duration: number = level.duration;
    let playing: boolean = false;

    function createGrid(level: Level) {
        // Copy of the available emojis for this level
        const copy = level.emojis.slice();
        const pairs: string[] = [];
        for (let i = 0; i < level.size ** 2 / 2; i++) {
            // Select a random index in the range of the length of the emojis array
            const index = Math.floor(Math.random() * copy.length);
            // Get the selected emoji
            const emoji = copy[index];
            // Remove the emoji from our copied array of emojis so we don't select it again 
            copy.splice(index, 1);
            // Add the emoji to our game set
            pairs.push(emoji);
        }

        // Duplicate each emoji so "pairs" actually holds pairs of emojis.
        pairs.push(...pairs);

        const emojiGrid = shuffle(pairs)
        // console.dir(emojiGrid)
        return emojiGrid
    }

    function countdown() {
        const start = Date.now()
        let remaining_at_start = remaining;

        function loop() {
            if (playing) return;

            requestAnimationFrame(loop);

            remaining = remaining_at_start - (Date.now() - start);

            if (remaining <= 0) {
                // TODO: Game has been lost
                playing = false;
            }
        }
        loop();
    }

    onMount(countdown);
</script>
<div class="game">
    
    <div class="info">
        <Countdown {remaining} duration={level.duration} />
    </div>
    <div class="grid-container">
        <Grid {grid} on:found={(e) => {
            found = [...found, e.detail.emoji]
        }}
        {found} 
        />
    </div>

    <div class="info">
        <Found {found} />
    </div>

</div>


<style>
    .game {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100%;
        font-size: min(1vmin, 0.5rem); /* HACK: */
    }

    .info {
        width: 80em;
        height: 10vmin;
    }

    .grid-container {
        width: 80em;
        height: 80em;
    }

</style>