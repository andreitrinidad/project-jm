<script>
  let theX, theY;
  let strength = 0.075;
  let parallaxWrapper;
  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry/i.test(navigator.userAgent);
  const isSafari = typeof(DeviceMotionEvent) !== 'undefined' && typeof(DeviceMotionEvent.requestPermission) === 'function';

  let isModalHidden = !isSafari;

  // let isModalHidden = false;

  // do the parallax thing
  function handleParallax(e) {
    e.stopPropagation();
    let x = Math.floor(e.clientX - parallaxWrapper.offsetLeft);
    let y = Math.floor(e.clientY - parallaxWrapper.offsetTop);
    let rotatedY =  Math.min(Math.max(parseInt(Math.floor(e.gamma * -1)), -45), 45);
    let rotatedX = Math.min(Math.max(parseInt(Math.floor(e.beta * -1)), -45), 45);

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

  //for iOS

  function requestDevicePermission() {
    if (isSafari) {
      DeviceOrientationEvent.requestPermission()
        .then(response => {
          if (response == 'granted') {
            console.alert('Thanks!');
            isModalHidden = true;
          }
        })
        .catch(console.error)
      isModalHidden = true;
    }
    else {
      alert('Wait, this isn\'t an iOS device');
    }
  }

  function hideModal() {
    isModalHidden = true;
  }
</script>

<svelte:window on:deviceorientation={handleParallax}/>

<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@300;500&display=swap" rel="stylesheet">
  <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🎉</text></svg>">
  <title>Happy birthday!</title>
</svelte:head>




<main on:mousemove={handleParallax}>
  <div class="lods" bind:this={parallaxWrapper}>
    <div data-depth="-0.95" class="parallax lods__elements"></div>
    <div data-depth="-0.15" class="parallax lods__clipper">
      <div data-depth="0.20" class="parallax lods__crush"></div>
    </div>
    <div data-depth="-0.15" class="parallax lods__bg"></div>
    <div data-depth="0.10" class="parallax lods__outline"></div>
  </div>

  <div class="texts">
    <h1>Happy Birthday <span> Jannie 🎉</span></h1>
    <h4>from yours truly—Andrei ✨</h4>
  </div>
	
  <!-- {#if isSafari} -->

  <div class="modal {isModalHidden && 'modal--hidden'}">
    <div>
      <h2>Oh snap! iOS is a special snowflake</h2>
      <h3>Can you allow some permissions for me?</h3>
      <button class="secondary" on:click={hideModal}>No</button>
      <button on:click={requestDevicePermission}>Sure!</button>
    </div>
  </div>
  <!-- {/if} -->


</main>

<style type="text/scss">
  @import './scss/breakpoint';

  :global(body), * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  //global 

  button {
    background-color: #333;
    color: white;
    padding: 20px 10px;
    border-radius: 20px;
    width: 100px;

      
    &.secondary {
      background-color: transparent;
      border: solid 1px #333;
      color: #333;
    }
  }

  .modal {
    position: fixed;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    width: 100vw;
    background: #f5f5f5;
    background-image: url('/images/texture.png');
    background-size: 1000px 1000px;
    z-index: 10;

    > div {
      padding: 50px;
      text-align: center;
    }

    h3 {
      margin-bottom: 30px;
    }

    &--hidden {
      display: none;
    }

    h2, h3 {
      font-weight: 300;
    }
  }

  .lods {
    position: relative;
    width: 90vw;
    height: 90vw;
    max-width: 500px;
    max-height: 500px;
    // text-align: center;

    &:after {
      position: absolute;
      content: '';
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      // background: white;
      // z-index: 7;

      // background-image: url('/images/texture2.png');
      // background-size: 100px 100px;
      // box-shadow: 0px 0px 50px rgba(black, 0.1);

    }



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
    height: 100vh;
    width: 100vw;
    font-family: 'Roboto Slab', serif;

    &:after {
      position: absolute;
      content: '';
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      background-image: url('/images/texture.png');
      background-size: 700px 700px;
      // background-blend-mode: screen;
      mix-blend-mode: overlay;
      pointer-events: none;
      z-index: 10;
    }

    div.texts {
      width: 90vw;
      max-width: 300px;
      text-align: center;

      h1 {
        font-weight: 300;
      }
      h4 {
        font-weight: 500;
        // text-align: right;
        margin-top: 20px;
      }
    }
  
  }
</style>