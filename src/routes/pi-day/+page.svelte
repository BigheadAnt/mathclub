<script lang="ts">
    import type {Point} from "../../types"
    import Target from "./Target.svelte"
    import Graph from "./Graph.svelte"

    const RTARGET = 250;
    const RDOT = 3;
    const TARGETVIEWSIDE = 2*RTARGET + 2*RDOT;
    const DELAY = 5;
    let dots: Point[] = [];
    let graphPoints: Point[] = [];
    const N = 100;
    const CENTER: Point = {x:RTARGET+RDOT,y:RTARGET+RDOT}
    const GRAPHWIDTH = 500
    const GRAPHHEIGHT = 200
    const GRAPHXUNIT = GRAPHWIDTH/N
    const GRAPHYUNIT = GRAPHHEIGHT/4
    let intervalid: NodeJS.Timer;
    
    function distance(p1: Point, p2: Point): number {
        const dx = p2.x - p1.x;
        const dy = p2.y - p1.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    function inside(p: Point, center: Point, r: number): boolean {
        return (distance(p, center) < r)
    }

    function getRandomIntInclusive(min: number, max: number): number {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    function handleClick() {
        let i = 0;
        dots = []; // do we need a check to see if dots is empty here
        graphPoints = [];
        clearInterval(intervalid)
        let insideCircle = 0;
        let outsideCircle = 0;
        intervalid = setInterval(() => {
            let x = getRandomIntInclusive(RDOT,2*RTARGET+RDOT);
            let y = getRandomIntInclusive(RDOT,2*RTARGET+RDOT);
            if (inside({x, y}, CENTER, RTARGET)) {
                insideCircle++;
            }
            else {
                outsideCircle++;
            }
            let pi = 4*insideCircle/(insideCircle + outsideCircle);
            dots.push({
                x,
                y,
                inside: inside({x,y}, CENTER, RTARGET)
            });
            graphPoints.push({
                x: GRAPHXUNIT * i,
                y: GRAPHHEIGHT - pi * GRAPHYUNIT,
                n: i+1,
                pi 
            })
            graphPoints = graphPoints;
            dots = dots;
            i++;
            if (i == N){
                clearInterval(intervalid);
            }
        }, DELAY)
    }

</script>

<div class="flex flex-col w-full h-screen items-center">
  
    <Target rtarget={RTARGET} rdot={RDOT} targetviewside={TARGETVIEWSIDE} dots={dots}> </Target>
    <button on:click={handleClick}>
      Start
    </button>
    <Graph graphPoints = {graphPoints} graphDimensions = {{w: GRAPHWIDTH, h: GRAPHHEIGHT}} units = {{x: GRAPHXUNIT, y: GRAPHYUNIT}}> </Graph>
</div>