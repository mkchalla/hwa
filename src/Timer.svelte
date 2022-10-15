<script>
    import { createEventDispatcher } from "svelte";
    import PorgressBar from "./PorgressBar.svelte";


    let dispatch = createEventDispatcher();

    let progress = 0;

    const totalSeconds = 20;
    let secondsLeft = totalSeconds;

    $: progress = ((totalSeconds - secondsLeft)/totalSeconds)*100;
    
    // function timer() {
    //     // time for the 20 seconds
    //     if (secondsLeft-- > 1){
    //         setTimeout(timer, 1000);
    //     }  
    // }

    // function start_timer() {
    //     secondsLeft = totalSeconds;
    //     // timer for the first second
    //     setTimeout(timer, 1000);
    // }

    let isRunning = false;
    function startTimer() {
        isRunning = true;
        const timer = setInterval(() => {
            secondsLeft -= 1;
            if (secondsLeft ==0) {
                clearInterval(timer);
                isRunning = false;
                secondsLeft = totalSeconds;
                dispatch('end')
            }
        }, 1000);
    }

</script>

<div bp="grid">
    <h2 bp="offset-5@md 4@md 12@sm">
        Seconds Left: {secondsLeft}
    </h2>
</div>

<PorgressBar {progress}/>

<div bp="grid">
    <button bp="offset-5@md 4@md 12@sm" 
            class="start" 
            disabled={isRunning}
            on:click={startTimer}>
        Start
    </button>
</div>


<style>
    h2 {
        margin: 0;
    }

    .start {
        background-color: rgb(154,73,73);
        width: 100%;
        margin-top: 10px 0;
    }
    .start[disabled] {
        background-color: rgb(194, 194, 194);
        cursor: not-allowed;
    }
</style>