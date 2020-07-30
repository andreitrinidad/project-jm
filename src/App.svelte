<script>
  
  // const parallaxElements = document.querySelectorAll('.parallax');
  let x, y;


  // binded container
  let parallaxWrapper;

  // do the parallax thing
  function handleMouseMove(e) {
    
    x = e.clientX;
    y = e.clientY;


    // get child elements
    const parallaxElements = parallaxWrapper.querySelectorAll('.parallax');

    parallaxElements.forEach(element => {
      const depth = e.dataset.depth;

      console.log('depth => ', depth);
    });

  }

  


</script>


<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@300;500&display=swap" rel="stylesheet">
</svelte:head>


<main on:mousemove={handleMouseMove}>
  <div class="lods" bind:this={parallaxWrapper}>
    <div data-depth="0.25" class="parallax lods__elements"></div>
    <div data-depth="0.25" class="parallax lods__crush"></div>
    <div data-depth="0.25" class="parallax lods__bg"></div>
    <div data-depth="0.25" class="parallax lods__outline"></div>
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
    }

    > div  {
      @extend %position;
    }

    &__elements {
      background-image: url('/images/elements.svg');
      z-index: 4;
    }

    &__crush {
      background-image: url('/images/her.svg');
      background-size: 400px 400px;
      z-index: 3;
      -webkit-mask-image: url('/images/clipper.svg');
      -webkit-mask-repeat: no-repeat;
      -webkit-mask-size: 90%;
      -webkit-mask-position: center;
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
    background: #f5f5f5;
    background-image: url('/images/texture.png');
    background-size: 1000px 1000px;
    height: 100%;
    width: 100%;
    font-family: 'Roboto Slab', serif;
  
    h1 {
      font-weight: 300;
    }
  }
</style>