<script lang="ts">
    import type {Point} from "../../types";
    export let graphPoints: Point[];
    export let graphDimensions: {w:number, h:number};
    export let units: {x:number, y:number};
    let paths: string[] = [];
    const MARGINS = {
        top: 50,
        bottom: 30,
        left: 30,
        right:30,
    };
    let l = 0;
    $: {
        l = graphPoints.length
        if (l == 0){
            paths = []
        }
        if (l >= 2) {
            paths.push(`M ${graphPoints[l-2].x + MARGINS.left} ${graphPoints[l-2].y + MARGINS.top} L ${graphPoints[l-1].x + MARGINS.left} ${graphPoints[l-1].y + MARGINS.top}`)
            paths = paths;
        }
    }

</script>

<svg style:width="{graphDimensions.w + MARGINS.left + MARGINS.right}px" style:height="{graphDimensions.h + MARGINS.top + MARGINS.bottom}px">
    <path stroke-width = "0.7" stroke = "black" fill="none"
    d="M {MARGINS.left} {MARGINS.top} L {MARGINS.left} {MARGINS.top + graphDimensions.h} L {MARGINS.left + graphDimensions.w} {MARGINS.top + graphDimensions.h}">
    </path>

    <path stroke-width = "0.7" stroke = "green"
    d="M {MARGINS.left} {graphDimensions.h - Math.PI*units.y + MARGINS.top} L {MARGINS.left + graphDimensions.w} {graphDimensions.h - Math.PI*units.y + MARGINS.top}">
    </path>
    {#each paths as line, i}
        <path d={line} stroke-width="0.5" stroke="black"></path>
    {/each}
    {#if l>=2}
    <text x="{graphPoints[l-1].x + MARGINS.left}" y="{MARGINS.top + graphDimensions.h + 20}" class = "text-sm" text-anchor = "middle">
        {graphPoints[l-1].n}
    </text>

    <text x="{graphPoints[l-1].x + MARGINS.left}" y="{MARGINS.top + graphPoints[l-1].y - 20}" class = "text-sm" text-anchor = "middle">
        {graphPoints[l-1].pi}
    </text>

    <path stroke-width = "0.2" stroke = "black"
    d="M {MARGINS.left+graphPoints[l-1].x} {MARGINS.top + graphPoints[l-1].y} L {MARGINS.left+graphPoints[l-1].x} {graphDimensions.h + MARGINS.top}">
    </path>
    {/if}
</svg>