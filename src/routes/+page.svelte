<script>
    import nepalMap from "$lib/assets/nepal-map.png";
    import "./app.css"; 
    import { fade } from "svelte/transition";
    
    const link="https://districtsofnepal.netlify.app/",
		fbLink=`https://facebook.com/share.php?u=${link}`,
		twitterLink=`https://twitter.com/share?url=${link}`,
		linkedinLink=`https://linkedin.com/sharing/share-offsite/?url=${link}`,
		redditLink=`https://reddit.com/submit?url=${link}`;

    let runAnimation=false;

    setTimeout(()=>{
        runAnimation=true
    },500)
</script>

<div class="main">
    <div class="header">
        <h2 class="title">
            <div class="title-up">
                <p class="t-blue">Districts of</p>
            </div>
            <div class="title-down">
                <img src={nepalMap} alt="Nepal Map">
                <p class="t-red">Nepal</p>
            </div>
        </h2>
        <div class="share">
            <li>
				<a href={fbLink} target="_blank"><i class="fa-brands fa-square-facebook blue-medias"></i></a>
			</li>
			<li>
				<a href={twitterLink} target="_blank"><i class="fa-brands fa-square-twitter green-medias"></i></a>
			</li>
			<li>
				<a href={linkedinLink} target="_blank"><i class="fa-brands fa-linkedin blue-medias"></i></a>
			</li>
			<li>
				<a href={redditLink} target="_blank"><i class="fa-brands fa-square-reddit green-medias"></i></a>
			</li>
        </div>
    </div>
    {#if runAnimation}
    <div transition:fade class="container">
        <div class="card">
            <div class="card-img" id="img-1"></div>
            <div class="card-txt">
                <p class="card-p">Type districts of Nepal</p>
                <a href="./all-districts/type" class="card-link">
                    <button class="play-btn">Play</button>
                </a>
            </div>
        </div>
        <div class="card">
            <div class="card-img" id="img-2"></div>
            <div class="card-txt">
                <p class="card-p">Click districts of Nepal</p>
                <a href="./all-districts/click" class="card-link">
                    <button class="play-btn">Play</button>
                </a>
            </div>
        </div>
    </div>
    <div transition:fade class="footer">
        <p class="credit">Created by <a href="https://linkedin.com/in/rubekk" target="_blank" rel="noreferrer">Rubek</a></p>
    </div>
    {/if}
</div>

<style>
    .header{
        padding: .75rem 2rem;
        background-color: #fff;
        border-bottom: 1px solid #dcdcdc;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .title{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-size: 1.75rem;
    }
    .title-down{
        margin-top: -10px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .title-down img{
        width: 70px;
        margin-right: .75rem;
        transform: rotate(-15deg);
    }
    .t-red{
        color: #ED143D;
    }
    .t-blue{
        color: #0059FF;
    }
    .container{
        margin: 5rem auto 0;
        padding: 3rem 3rem;
        width: max-content;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        row-gap: 1rem;
        column-gap: .5rem;
        background-color: #fff;
        border-radius: 3px;
        border: 1px solid #dcdcdc;
    }
    .card{
        width: 300px;
        background-color: #fff;
        border: 1px solid #dcdcdc;
        border-radius: 3px;
        box-shadow: 1px 1px 1px rgb(46 128 187 / 20%);
    }
    .card-img{
        width: 100%;
        height: 175px;
        background-position: center;
        background-size: cover;
        background-repeat: no-repeat;
    } 
    .card-img:hover{
        transform: scale(1.01);
    }
    #img-1{
        background-image: url($lib/assets/type.svg);
    }
    #img-2{
        background-image: url($lib/assets/click.svg);
    }
    .card-txt{
        padding: 1.25rem 1rem;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .card-link{
        color: #000;
    }
    .play-btn{
        padding: 0.5rem 1rem;
        font-style: italic;
        background-color: crimson;
        background-image: url($lib/assets/redbg.png);
        color: #fff;
        border: none;
        border-radius: 3px;
        transition: .3s;
    }
    .play-btn:hover{
        color: rgb(183, 189, 189);
    }
    .credit{
        margin: 5rem 0 1rem;
        text-align: center;
    }
    .credit a{
        font-style: italic;
        text-decoration: underline;
        color: #0059FF;
    }

    /* media queries */
    @media (max-width: 750px){
        .container{
            margin-top: 3rem;
            flex-direction: column;
        }
        .credit{
            margin-top: 3rem;
        }
    }
    @media (max-width: 450px){
        .title{
            font-size: 1.25rem;
        }
        .title-down{
            margin-top: -5px;
        }
        .title-down img{
            width: 50px;
        }
        .container{
            padding: 3rem 2rem;
        }
    }
    @media (max-width: 415px){
        .container{
            padding: 3rem 1rem;
        }
    }
    @media (max-width: 400px){
        .header{
            padding: .75rem 1rem;
        }
        .card{
            width: 250px;
        }   
    }
    @media (max-width: 325px){
        .card{
            width: 225px;
        }
        .card-p{
            font-size: .9rem;
        }
    }
</style>