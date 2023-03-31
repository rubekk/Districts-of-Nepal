<script>
    import RouteHeader from "./../../components/RouteHeader.svelte";
	import ClickMap from "./../../components/ClickMap.svelte";
    import { districts, displayedDistricts, currDistrict, changeCurrDistrict, currClickHoverDistrict } from "./../../stores/stores";
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
        presClickHoverDistrict,
        started=false,
        submitted=false,
        mobileTxt=false,
        showDistricts=false,
        runAnimation=false;
    
    const firebaseApp=initializeApp(firebaseConfig);
	const db=getFirestore();

    setTimeout(()=>{
        runAnimation=true
    },500)

    districts.subscribe(value => allDistricts=value);
    displayedDistricts.subscribe(value => prevDistricts=value);
    currDistrict.subscribe(value => presDistrict=value);
    currClickHoverDistrict.subscribe(value => presClickHoverDistrict=value);

    onMount(async ()=>handleResize());

    const handleStartSubmit=()=>{
        if(!started) { //when start button is clicked
            started=true;
            submitted=false;
            showDistricts=false;
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
                showDistricts=true;
                presDistrict="Your score is "+score+"/77";
                changeCurrDistrict.set(false);

                addToFirebase()
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
    const addToFirebase=async ()=>{
        await addDoc(collection(db, "click-districts"), {
            districts: prevDistricts,
            score: score,
            createdAt: new Date()
        })
    }

    $: {
        if(prevDistricts.length==77) {
            started=false;
            submitted=true;
            showDistricts=true;
            presDistrict="Your score is "+score+"/77";
            changeCurrDistrict.set(false);

            addToFirebase();
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
            <ClickMap showDistricts={showDistricts}/>
            {#if presClickHoverDistrict && showDistricts}
                <div class="hover-district">{presClickHoverDistrict}</div>
            {/if}
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