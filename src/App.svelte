<script>
  let theX, theY;
  let strength = 0.075;
  let parallaxWrapper;

  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry/i.test(navigator.userAgent);

  console.log(isMobile);


  // do the parallax thing
  function handleParallax(e) {

    e.stopPropagation();

  
    
    let x = Math.floor(e.clientX - parallaxWrapper.offsetLeft);
    let y = Math.floor(e.clientY - parallaxWrapper.offsetTop);

    let rotatedY =  Math.min(Math.max(parseInt(Math.floor(e.gamma * -1)), -45), 45);
    let rotatedX = Math.min(Math.max(parseInt(Math.floor(e.beta * -1)), -45), 45);

    // console.log('rotatedYinvert => ',rotatedY * -1);
    // console.log('rotatedY => ',rotatedY);

    // get child elements
    const parallaxElements = parallaxWrapper.querySelectorAll('.parallax');
    const containerWidth = parallaxWrapper.offsetWidth;
    const containerHeight = parallaxWrapper.offsetHeight;

    parallaxElements.forEach(element => {
      const depth = element.dataset.depth;
      const moveX = isMobile ? ((containerWidth/2) * rotatedY) / 400 : ((containerWidth / 2) - x) * (strength * depth);
      const moveY = isMobile ? ((containerWidth/2) * rotatedX) / 700 : ((containerHeight / 2) - y) * (strength * depth);
      element.style.transform = `translate3d(${moveX}px, ${moveY}px, 0)`;
    });
  }
</script>

<svelte:window on:deviceorientation={handleParallax}/>

<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@300;500&display=swap" rel="stylesheet">
</svelte:head>

<main on:mousemove={handleParallax}>
  <div class="lods" bind:this={parallaxWrapper}>
    <div data-depth="-0.45" class="parallax lods__elements"></div>
    
    <div data-depth="-0.15" class="parallax lods__clipper">
      <div data-depth="0.20" class="parallax lods__crush"></div>
    </div>
    

    <div data-depth="-0.15" class="parallax lods__bg"></div>
    <div data-depth="-0.10" class="parallax lods__outline"></div>
  </div>
	<h1>Awit sayo lods x</h1>

</main>

<style type="text/scss">
  @import './scss/breakpoint';

  :global(body), * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .lods {
    position: relative;
    width: 90%;
    height: 90%;
    max-width: 500px;
    max-height: 500px;

    // &:before {
    //   position: absolute;
    //   content: '';
    //   top: -40px;
    //   left: -40px;
    //   right: -40px;
    //   bottom: -40px;
    //   // background: white;
    //   // z-index: -1;

    //   // background-image: url('/images/texture2.png');
    //   // background-size: 1000px 1000px;
    //   // box-shadow: 0px 0px 50px rgba(black, 0.1);

    // }



    %position {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-repeat: no-repeat;
      background-position: center center;
      transition: transform .4s ease-out;
    }

    > div  {
      @extend %position;
   
    }

    &__elements {
      background-image: url('/images/elements.svg');
      background-size: 80%;
      
      z-index: 5;
    }


    &__clipper{
      
      // background-color: red;
      -webkit-mask-image: url('/images/clipper.svg');
      -webkit-mask-repeat: no-repeat;
      -webkit-mask-size: 80%;
      -webkit-mask-position: center;
      z-index: 4;
    }


    &__crush {
      @extend %position;

      background-image: url('/images/her.svg');
      background-size: 40% auto;
      z-index: 3;
    }




    &__bg {
      background-image: url('/images/bg.svg');
      background-size: 80%;
      z-index: 2;
    }

    &__outline {
      background-image: url('/images/outline.svg');
      background-size: 90%;
      
      z-index: 1;
    }
  }

  main {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    background: #f5f5f5;
    background-image: url('/images/texture.png');
    background-size: 1000px 1000px;
    height: 100vh;
    width: 100vw;
    font-family: 'Roboto Slab', serif;
  
    h1 {
      font-weight: 300;
    }
  }
</style>