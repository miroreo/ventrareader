<script lang="ts">
    import touchBelow from "$lib/screens/touchBelow.svg";
    import processing from "$lib/screens/processing.svg";
    import go from "$lib/screens/go.svg";
    import insufficient from "$lib/screens/insufficient.svg";
    import notread from "$lib/screens/notread.svg";

    import logo from "$lib/screens/ventralogo.png";
    import taptopay from "$lib/screens/taptopay.png";

    import ventraGo from "$lib/sounds/ventraGo.mp3";
    import ventraStop from "$lib/sounds/ventraStop.mp3";
    import ventraWait from "$lib/sounds/ventraWait.mp3";

    let audioGo: HTMLMediaElement;
    let audioStop: HTMLMediaElement;
    let audioWait: HTMLMediaElement;

    let dots = $state([1,0,1,0]);
    let currentScreen = $state(touchBelow);
    let running = $state(false);
    const run = async () => {
        if(running) return;
        running = true;
        await audioWait.play();
        dots = [1, 1, 0, 0];
        currentScreen = processing;
        let seed = Math.random();
        setTimeout(async () => {
            if(seed < .6) {
                dots = [1,0,1,0];
                await audioGo.play();
                currentScreen = go;
            }
            if(seed >= .6 && seed < .9) {
                dots = [1,1,0,0];
                await audioStop.play();
                currentScreen = notread
            }
            if(seed >= .9) {
                dots = [1,1,0,0];
                await audioStop.play();
                currentScreen = insufficient;
            }
            setTimeout(() => {
                dots = [1,0,1,0];
                currentScreen = touchBelow;
                running = false;
            }, 2000);
        }, seed*200 + 500)
        
    }
</script>
<div class="min-h-screen max-h-screen sm:mx-auto bg-black flex flex-col text-center">
    <div class="dots flex flex-row gap-x-10 mx-auto">
        <div class="dot bg-blue-600"></div>
        <div class={"dot " + (dots[1] ? "bg-yellow-500" : "bg-gray-600")}></div>
        <div class={"dot " + (dots[2] ? "bg-green-500" : "bg-gray-600")}></div>
        <div class="dot bg-gray-600"></div>
    </div>
    <div class="screen">
        <img src={currentScreen}/>
    </div>
    <button class="tap flex flex-col items-center" onclick={run}>
        <div class="ventraBlue pt-5 px-5 rounded-tl-3xl rounded-tr-3xl w-full">
            <img src={logo} class="p-5 mx-auto"/>
        </div>
        <div class="bg-black pb-5 px-5 rounded-bl-3xl rounded-br-3xl w-full">
            <img src={taptopay} class="p-16" />
        </div>
    </button>
    <audio src={ventraGo} bind:this={audioGo} ></audio>
    <audio src={ventraStop} bind:this={audioStop} ></audio>
    <audio src={ventraWait} bind:this={audioWait} ></audio>
</div>
<style>
    :root {
        --ventraBlue: rgba(0,181,224,255);
    }
    .dots {
        @apply py-5;
    }
    .dot {
        @apply p-3 rounded-full;
    }
    .tap {
        @apply flex flex-col w-80 bg-gray-700 mx-auto my-auto h-full border-white border-2 rounded-3xl place-content-evenly;
    }
    .ventraBlue {
        background-color: var(--ventraBlue);
    }
</style>
