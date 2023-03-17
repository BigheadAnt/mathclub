<script lang="ts">
    import type {Point} from "../../types"
    import { fly } from 'svelte/transition';
    import { sineIn, expoOut } from 'svelte/easing'
    export let rtarget: number;
    export let rdot: number;
    export let targetviewside: number;
    export let dots: Point[];
    const CENTER: Point = {x:rtarget+rdot,y:rtarget+rdot}

   

</script>

<svg style:width="{2*rtarget+2*rdot}px" style:height="{2*rtarget+2*rdot}px">
    <rect class="stroke-current fill-white" x="{rdot}" y="{rdot}" width="{2*rtarget}px" height="{2*rtarget}px" />
    <circle class="stroke-current fill-white" cx={CENTER.x} cy={CENTER.y} r={rtarget} />
    {#each dots as dot, i}
            <circle stroke-width="0.2"
            in:fly="{{ x:-dot.x, y:2*rtarget-dot.y, duration:500, easing: sineIn, opacity:0, delay: 0}}"  
            fill={dot.inside ? "blue":"orange"} cx = {dot.x} cy = {dot.y} r={rdot} />
    {/each}
</svg>
