<script>
    import RouteHeader from "./../../components/RouteHeader.svelte";
	import ClickMap from "./../../components/ClickMap.svelte";
    import { districts, displayedDistricts, currDistrict, changeCurrDistrict } from "./../../stores/stores";
    import { onMount } from "svelte";
    import { fade } from "svelte/transition";
    import { initializeApp } from "firebase/app";
	import { getFirestore, collection, addDoc } from "firebase/firestore";
    import { firebaseConfig } from "$lib/firebaseConfig";
    import "./../../app.css";
    import "./../route.css";

    let score=0, 
        allDistricts=[],
        prevDistricts=[],
        presDistrict="",
        started=false,
        submitted=false,
        mobileTxt=false,
        runAnimation=false;
    
    const firebaseApp=initializeApp(firebaseConfig);
	const db=getFirestore();

    setTimeout(()=>{
        runAnimation=true
    },500)

    districts.subscribe(value=>{
        allDistricts=value;
    })
    displayedDistricts.subscribe(value=>{
        prevDistricts=value;
    })
    currDistrict.subscribe(value=>{
        presDistrict=value;
    })

    onMount(async ()=>handleResize());

    const handleStartSubmit=async ()=>{
        if(!started) { //when start button is clicked
            started=true;
            submitted=false;
            prevDistricts=[];
            displayedDistricts.set([]);
            changeCurrDistrict.set(true);
            
            presDistrict=allDistricts[Math.floor(Math.random()*allDistricts.length)];
            currDistrict.set(presDistrict);
        }
        else{ //when submit button is clicked and prevDistricts is greater or equal to 1
            if(prevDistricts.length>0){
                started=false;
                submitted=true;
                presDistrict="Your score is "+score+"/77";
                changeCurrDistrict.set(false);

                await addDoc(collection(db, "click-districts"), {
                    districts: prevDistricts,
                    score: score,
                    createdAt: new Date()
                })
            } 
        }
    }
    const capitalizeDistrict=district=>{
        return district.slice(0,1).toUpperCase()+district.slice(1,district.length);
    }
    const newLoad=()=>{
        started=false;
        submitted=false;
        displayedDistricts.set([]);
        currDistrict.set("");
    }
    const handleResize=()=>{
        mobileTxt=window.innerWidth>450?false:true;
    }

    $: {
        if(prevDistricts.length==77) {
            started=false;
            submitted=true;
            presDistrict="Your score is "+score+"/77";
            changeCurrDistrict.set(false);
        }
        score=0;
        prevDistricts.forEach(item=>{
            if(item.correct) score++;
        })
    }
</script>

<svelte:window on:resize={handleResize}></svelte:window>

<div class="main">
    <RouteHeader pageTitle={mobileTxt?"Click":"Click districts of Nepal"}/>
    {#if runAnimation}
    <div transition:fade use:newLoad class="container">
        <div transition:fade class="map">
            <ClickMap/>
        </div>
        <div class="district-btn">
            <span class="score"><span class="score-txt">Score:</span> {score}</span>
            <p style={submitted?"font-size: 1.5rem; font-style: italic; color: #797979":""}>{capitalizeDistrict(presDistrict)}</p>
            <button on:click={handleStartSubmit} class="start-submit">{!started?"Start":"Submit"}</button>
        </div>
        <div class="districts" style={prevDistricts.length==0?"flex-direction: column; align-items: center;":""}>
            {#if prevDistricts.length>0}
                {#each prevDistricts as item}
                    <span style={item.correct?"":"text-decoration: line-through; color: #000;"}>{capitalizeDistrict(item.district)}</span>
                {/each}
            {:else}
                <div class="emojis">
                    <i class="fa-solid fa-face-frown-open"></i>
                    <i class="fa-solid fa-face-frown-open"></i>
                    <i class="fa-solid fa-face-frown-open"></i>
                </div>
                <p>No districts guessed. {started?"Click a district":"Start and click a district."}</p>
            {/if} 
        </div>
    </div>
    {/if}
</div>