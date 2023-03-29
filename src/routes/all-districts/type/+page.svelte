<script>
    import RouteHeader from "./../../components/RouteHeader.svelte";
    import TypeMap from "../../components/TypeMap.svelte";
    import redBg from "$lib/assets/redbg.svg";
    import { onMount } from "svelte";
    import "./../../app.css";
    import "./../route.css";

    const districts=['achham', 'arghakhanchi', 'baglung', 'baitadi', 'bajhang', 'bajura', 'banke', 'bara', 'bardiya', 'bhaktapur', 'bhojpur', 'chitwan', 'dadeldhura', 'dailekh', 'dang', 'darchula', 'dhading', 'dhankuta', 'dhanusha', 'dolakha', 'dolpa', 'doti', 'gorkha', 'gulmi', 'humla', 'ilam', 'jajarkot', 'jhapa', 'jumla', 'kailali', 'kalikot', 'kanchanpur', 'kapilvastu', 'kaski', 'kathmandu', 'kavre', 'khotang', 'lalitpur', 'lamjung', 'mahottari', 'makwanpur', 'manang', 'morang', 'mugu', 'mustang', 'myagdi', 'nawalparasi east','nawalparasi west', 'nuwakot', 'okhaldhunga', 'palpa', 'panchthar', 'parbat', 'parsa', 'pyuthan', 'ramechhap', 'rasuwa', 'rautahat', 'rolpa', 'rukum east', 'rukum west', 'rupandehi', 'salyan', 'sankhuwasabha', 'saptari', 'sarlahi', 'sindhuli', 'sindhupalchowk', 'siraha', 'solukhumbu', 'sunsari', 'surkhet', 'syangja', 'tanahun', 'taplejung', 'terhathum', 'udayapur'];

    let userDistricts=[],
        currDistrict,
        score=0,
        seconds=0,
        minutes=5,
        started=false,
        submited=false,
        mobileTxt=false,
        interval,
        inputElem;
    
    onMount(async ()=>handleResize());

    const handleInput=()=>{
        if(districts.includes(currDistrict.toLowerCase()) && !userDistricts.includes(currDistrict.toLowerCase())){
            userDistricts.push(currDistrict.toLowerCase());
            userDistricts=[...userDistricts];
            currDistrict="";
        }   
        score=userDistricts.length;
    }
    const handleStartSubmit=()=>{
        if(!started && !submited){
            started=true;
            seconds=0;
            minutes=5;
            startTime();
        }
        else if(started && score==0){
            inputElem.placeholder="Type a district";
            setTimeout(()=>{
                inputElem.placeholder="Enter districts...";
            },1500)
        }
        else if(started && score>0){
            started=false;
            submited=true;
            clearInterval(interval);
            inputElem.placeholder="Your score is "+score+" / 77";
            inputElem.style.fontSize="1.5rem";
        }
        else if(submited){
            started=true;
            submited=false;
            userDistricts=[];
            currDistrict="";
            inputElem.placeholder="Enter districts...";
            inputElem.style.fontSize="1rem";
            score=0;
            seconds=0;
            minutes=5;
            startTime();
        }
    }
    const startTime=()=>{
        interval=setInterval(()=>{
            if(seconds==0) {
                seconds=minutes>0?59:0;
                if (minutes>0) minutes--;
            }
            else seconds--;

            if(seconds==0 && minutes==0){
                started=false;
                submited=true;

                if(score>0 && inputElem){
                    inputElem.placeholder="Your score is "+score+" / 77";
                    inputElem.style.fontSize="1.5rem";
                }
                clearInterval(interval);
            }
        },1000);
    }
    const handleResize=()=>{
        mobileTxt=window.innerWidth>450?false:true;
    }
</script>

<svelte:window on:resize={handleResize}></svelte:window>

<div class="main">
    <RouteHeader pageTitle={mobileTxt?"Click":"Click districts of Nepal"}/>
    <div class="container">
        <div class="map">
            <TypeMap districts={districts} userDistricts={userDistricts}/>
            <div class="time">{minutes.toString().length==1?`0${minutes}`:minutes}:{seconds.toString().length==1?`0${seconds}`:seconds}</div>
        </div>
        <div class="inp">
            <div class="score"><span class="score-txt">Score: </span>{score}</div>
            <input type="text" bind:this={inputElem} bind:value={currDistrict} on:input={handleInput} placeholder="Enter districts..." disabled={!started}>
            <button on:click={handleStartSubmit} class="start-submit">{!started?"Start":score>0?"Submit":"Started"}</button>
        </div>
        <div class="districts" style={userDistricts.length==0?"flex-direction: column; align-items: center;":""}>
            {#if userDistricts.length>0}
                {#each userDistricts as district}
                    <span>{district}</span>
                {/each}
                {#if submited}
                {#each districts as district}
                {#if !userDistricts.includes(district)}
                    <span style={"text-decoration: line-through; color: #000;"}>{district}</span>
                {/if}
                {/each}
                {/if}
            {:else}
                <div class="emojis">
                    <i class="fa-solid fa-face-frown-open"></i>
                    <i class="fa-solid fa-face-frown-open"></i>
                    <i class="fa-solid fa-face-frown-open"></i>
                </div>
                <p>No districts guessed. {started?"Type a district":"Start and type a district."}</p>
            {/if} 
        </div>
    </div>
</div>

<style>
    .map{
        position: relative;
    }
    input{
        border: none;
        font-style: italic;
        outline: none;
    }
    .time{
        padding: 1rem 2rem;
        position: absolute;
        top: 0;
        right: 0;
        font-size: 1.2rem;
        color: #fff;
        background-color: crimson;
        background-image: url($lib/assets/redbg.png);
    }

    /* media queries */
    @media (max-width: 750px){
        .time{
            padding: .5rem 1rem;
        }
    }
    @media (max-width: 525px){
        .time{
            padding: .5rem;
        }
    }
    @media(max-width: 400px){
        .time{
            font-size: .9rem;
        }
    }
</style>