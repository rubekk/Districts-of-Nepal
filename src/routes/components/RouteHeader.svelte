<script>
    export let pageTitle;

    const link="https://districtsofnepal.netlify.app/",
		fbLink=`https://facebook.com/share.php?u=${link}`,
		twitterLink=`https://twitter.com/share?url=${link}`,
		linkedinLink=`https://linkedin.com/sharing/share-offsite/?url=${link}`,
		redditLink=`https://reddit.com/submit?url=${link}`;
</script>

<div class="header">
    <div class="left">
        <a href="/" class="home-link">
            <i class="fa-solid fa-arrow-left"></i>
        </a>
        <span>{pageTitle}</span>
    </div>
    <div class="right">
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
</div>

<style>
    .header{
        padding: 1.5rem 2rem;
        background-color: #fff;
        border-bottom: 1px solid #dcdcdc;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 1.25rem;
    }
    .home-link{
        margin-right: 1rem;
    }
    
    /* media queries */
    @media (max-width: 500px){
        .header{
            padding: 1.5rem 1rem;
        }
        .home-link{
            margin-right: .25rem;
        }
    }
</style>