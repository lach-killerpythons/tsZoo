<script>
	import { tweened } from 'svelte/motion';
    import Typewriter from "svelte-typewriter";
    //let original = 5 * 60; // = 5min
    let original = 10; // TYPE NUMBER OF SECONDS HERE
	let timer = tweened(original)

    //let resetMode = true;

  // ------ dont need to modify code below

    setInterval(() => {
        if ($timer > 0) $timer--;
    }, 1000);

    $: minutes = Math.floor($timer / 60);
    $: minname = minutes > 1 ? "mins" : "min";
    $: seconds = Math.floor($timer - minutes * 60)

    function resetTimer() {
        $timer = original;
    }

</script>

<main>


  {#if $timer < 1}

    <Typewriter loop>
      <h1>Starting soon....</h1>
    </Typewriter>
  {:else}
  
<h1>Time Left is: <span class="mins">{minutes}</span>{minname} 
	
	<span class="secs">{seconds}</span>s!</h1>
<progress value={$timer/original}></progress>
  {/if}


</main>

<style>
  main {
    width: 600px;
    margin: 0 auto;
  }
	
	progress {
		display: block;
		width: 100%;
	}
	.mins {
		color: darkgoldenrod;
	}
	.secs {
		color: darkgoldenrod;
	}

</style>
