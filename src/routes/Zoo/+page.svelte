<svelte:head>
    <title>Typescript Zoo</title>
</svelte:head>

<script lang="ts">
    import { onMount } from "svelte";
    import MyTimer from './MyTimer.svelte';
    import MyLogo from './MyLogo.svelte';

    // states
    let myInterval: number | null | undefined = null;
    let randomMovement: boolean = false;
    let timerOn: boolean = false;

    
    // "sprites"
    let wildAnimal: string = "🦘"
    let emptySquare: string = "🟩"


    // grid vars
    let lines: Array<string> = [];
    let nLines: number = 5;
    let nSpaces: number = 20;
    let currentX: number = 0;
    let currentY: number = 0;
    
    // render field
    for (let s = 0; s < nSpaces; s++ )
    {
        lines.push(emptySquare)
    }
    let lineBlok: Array<any> = []
    for (let i = 0; i <= nLines; i++)
    {
        let cloneLine: Array<any> = Object.create(lines);
        cloneLine.push(i.toString());
        lineBlok.push(cloneLine)
    }

    enum Direction {
        Up,
        Down,
        Left,
        Right,
    }

    // manual movement 
    function moveMe(nextMove: Direction) {
        //let deltaX: number = 0;
        //let deltaY: number = 0;

        console.log(currentX, currentY);

        switch(nextMove) {
            case Direction.Up:
                //deltaY = -1;
                //console.log(deltaY);
                movePosition2(currentX, currentY-1);
                return;
            case Direction.Down:
                //deltaY = 1;
                movePosition2(currentX, currentY+1);
                return;
            case Direction.Left:
                movePosition2(currentX-1, currentY);
                return;
            case Direction.Right:
                //deltaX = 1;
                movePosition2(currentX+1, currentY);
                return;
        }
    }

    function movePosition2(x: number, y: number) {
        
        if(x < nSpaces && y <= nLines && x >= 0 && y >= 0)
        {
            currentX = x;
            currentY = y;
            clear();
            lineBlok[y][x] = wildAnimal;
        }
        else {
            console.log("move out of bounds");
        }
        //return 0;
    }

    function getRandomInt(max: number) {
        return Math.floor(Math.random() * max);
    }

    function randPos() {
        currentX = getRandomInt(nSpaces-1)
        currentY = getRandomInt(6)
        lineBlok[currentY][currentX] = wildAnimal;
        
    }

    function clear() {
        //clear lineblock
        lineBlok = [];
        for (let i = 0; i <= nLines; i++){
            // create new object (not reference)
            let cloneLine: Array<any> = Object.create(lines);
            lineBlok.push(cloneLine)
        }
    }

    function reset() {

        clear();
        currentX = 0;
        currentY = 0;
    }
    
    function moveRandom() {
        clear();
        randPos();
    }

// window can only be accessed in browser not server 
onMount(() => {
    window.addEventListener('keydown', function(e) {
        
        let pressedKey = e.code;
        if (pressedKey == 'KeyA' || pressedKey == 'KeyD' || pressedKey == 'KeyS' || pressedKey == 'KeyW')
        {
            switch(pressedKey){
                case 'KeyA':
                    moveMe(Direction.Left);
                    return;
                case 'KeyD':
                    moveMe(Direction.Right);
                    return;
                case 'KeyW':
                    moveMe(Direction.Up);
                    return;
                case 'KeyS':
                    moveMe(Direction.Down);
                    return;
            }
        }
        
    });
});



function toggleRandom() {

    randomMovement = !randomMovement
    if(randomMovement){
        myInterval = setInterval(() => moveRandom(), 1000);
    } else {
        window.clearInterval(myInterval) // error message for any type
    }
}


</script>

<MyLogo/>


{#if timerOn}
<MyTimer/>
{/if}

{#each lineBlok as line, index}
    <p>{line.join('')} - {index}</p>
{/each}
<br>

<button on:click={() => toggleRandom()}> random jump: {randomMovement}</button>
<button on:click={() => (reset())}> clear </button>
<br>
<h2>You can also move with WASD!</h2>
<button on:click={() => (moveMe(Direction.Up))}> Up </button>
<button on:click={() => (moveMe(Direction.Down))}> Down </button>
<button on:click={() => (moveMe(Direction.Left))}> Left </button>
<button on:click={() => (moveMe(Direction.Right))}> Right </button>
<br>
<button on:click={() => timerOn = !timerOn}> timer on : {timerOn} </button>

