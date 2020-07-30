<script>

  let x, y;


  let strength = 0.075;


  // binded container
  let parallaxWrapper;
  let mainWrapper;


  // x = mainWrapper.clientX;
  // y = mainWrapper.clientY;


  // do the parallax thing
  function handleMouseMove(e) {
    e.stopPropagation();
    
    x = Math.floor(e.clientX - parallaxWrapper.offsetLeft);
    y = Math.floor(e.clientY - parallaxWrapper.offsetTop);

    //  Math.floor(e.clientX - $(this).offset().left)

    // get child elements
    const parallaxElements = parallaxWrapper.querySelectorAll('.parallax');

    const containerWidth = parallaxWrapper.offsetWidth;
    const containerHeight = parallaxWrapper.offsetHeight;

    console.log('y => ', parallaxWrapper.offsetHeight);

    

    parallaxElements.forEach(element => {
      const depth = element.dataset.depth;
      const moveX = ((containerWidth / 2) - x) * (strength * depth);
      const moveY = ((containerHeight / 2) - y) * (strength * depth);

      // console.log('moveX => ', moveX);
      // element.innerHTML = moveX;

      element.style.transform = `translate3d(${moveX}px, ${moveY}px, 0)`;
    });

  }



  


</script>


<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@300;500&display=swap" rel="stylesheet">
</svelte:head>


<main on:mousemove={handleMouseMove} bind:this={mainWrapper}>
  <div class="lods" bind:this={parallaxWrapper}>
    <div data-depth="0.45" class="parallax lods__elements"></div>
    
    <div data-depth="0.15" class="parallax lods__clipper">
      <div data-depth="-0.20" class="parallax lods__crush"></div>
    </div>
    

    <div data-depth="0.15" class="parallax lods__bg"></div>
    <div data-depth="0.10" class="parallax lods__outline"></div>
  </div>
	<h1>Awit sayo lods</h1>
</main>

<style type="text/scss">
  :global(body), * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .lods {
    position: relative;
    width: 500px;
    height: 500px;

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
      z-index: 5;
    }


    &__clipper{
      // background-color: red;
      -webkit-mask-image: url('/images/clipper.svg');
      -webkit-mask-repeat: no-repeat;
      -webkit-mask-size: 90%;
      -webkit-mask-position: center;
      z-index: 4
    }


    &__crush {
      @extend %position;

      background-image: url('/images/her.svg');
      background-size: 400px 400px;
      z-index: 3;
    }




    &__bg {
      background-image: url('/images/bg.svg');
      background-size: 90%;
      z-index: 2;
    }

    &__outline {
      background-image: url('/images/outline.svg');
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