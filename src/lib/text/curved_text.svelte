<script module>
    let count = 0;
    function getId() {
        count++;
        return `curved-text-${count}`;
    }
</script>

<script lang="ts">
	import { onMount } from "svelte";


    let {displayText, font = "50px serif", gap=0.2, radius=200} = $props();

    const id = getId();

    let canvas: HTMLCanvasElement | null = null;
    let context: CanvasRenderingContext2D | null = null;

    onMount(() => {
        if (!canvas) return;
        context = canvas.getContext("2d");
        if (!context) return;

        context.font = font;
        context.textAlign = "center";
        context.fillStyle = "black";
        context.textBaseline = "middle";

        

        context.translate(canvas.width / 2 - parseInt(font.substring(0, 2)), canvas.height / 2 + radius - 50);

        let displayTextLength =  context.measureText(displayText).width

        let angle = displayTextLength * (1+gap) / radius;

        context.rotate(-angle/2);

        for (let i = 0; i < displayText.length; i++) {
            const char = displayText[i];
            context.rotate(angle * (context.measureText(char).width / displayTextLength));
            context.save();
            context.translate(0, -1 * radius);
            context.fillText(char, 0, 0);
            context.restore();
        }
    });
</script>

<canvas class="curved-text"
        bind:this={canvas} id={id}
        width = 1150
        height = 450></canvas>

