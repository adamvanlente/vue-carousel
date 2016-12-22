<template>
  <div class="sample">
    <h2>{{ title }}</h2>

    <!-- Main carousel wrapper -->
    <div id="carousel-wrapper" class="carousel-wrapper">

      <!-- Carousel navigation -->
      <div class="carousel-nav">
        <nav class="carousel-nav-left-arrow" v-on:click="navigateLeft()">&#x3c;</nav>
        <nav class="carousel-nav-right-arrow" v-on:click="navigateRight()">&#x3e;</nav>
        <nav class="nav-button-wrapper">
          <label class="nav-button"
                 v-for="(imageUrl, index) in imageArray"
                 v-on:click="navigateToSlide(index)"
                 v-bind:class="{ active: currentImageIndex == index }">
          </label>
        </nav>
      </div>
      <!-- END Carousel navigation -->

      <!-- Carousel content -->
      <div id="carousel-content" class="carousel-content">
        <img v-for="imageUrl in imageArray" v-bind:src="imageUrl">
      </div>
      <!-- END Carousel content -->

    </div>
    <!-- END Main carousel wrapper -->

  </div>
</template>

<script>

  // Import image data.
  import sampleImageData from '../js/sample-images';

  export default {
    data() {
      return {
        title: 'Carousel demo',

        // Currently viewing image.
        currentImageIndex: 0,

        // Min number of images in a random array.
        randomArrayMinImages: 3,

        // Max number of images in carousel, whether random or user assinged.
        arrayMaxImages: 7,

        // Number of seconds between automated carousel nav.
        secondsBetweenSlides: 5,
      };
    },
    mounted() {
      // Set position on window resize to avoid odd appearance.
      window.addEventListener('resize', this.setCarouselPosition);

      // Update the carousel at regular intervals.
      window.setInterval(() => {
        this.navigateRight();
      }, this.secondsBetweenSlides * 1000);
    },
    computed: {
      imageArray() {
        // Return image array, limited in size per arrayMaxImages.
        return this.setImageArray().slice(1, this.arrayMaxImages);
      },
    },
    methods: {
      /*
       * Image methods
       *
       */

      // Set image array can take an array of URLs, or will fetch
      // a random array from the sample data.
      setImageArray(imageArray) {
        if (imageArray) return imageArray;
        return this.getRandomArrayOfImages();
      },

      // Creates a random array of images.
      getRandomArrayOfImages() {
        const randomImageArray = [];
        const usedImageIndexes = {};
        const arrLen = this.getRandomArrayLength();
        while (randomImageArray.length < arrLen) {
          const randomImageIndex = this.randomInt(0, sampleImageData.length - 1);
          if (!usedImageIndexes[randomImageIndex]) {
            usedImageIndexes[randomImageIndex] = true;
            randomImageArray.push(sampleImageData[randomImageIndex]);
          }
        }

        return randomImageArray;
      },

      // Create a random length, which will determine the size of the
      // sample image array.
      getRandomArrayLength() {
        return this.randomInt(
          this.randomArrayMinImages, this.arrayMaxImages);
      },

      /*
       * Navigation functions
       *
       */
      navigateLeft() {
        this.navigateToSlide(this.currentImageIndex -= 1);
      },

      navigateRight() {
        this.navigateToSlide(this.currentImageIndex += 1);
      },

      navigateToSlide(index) {
        this.currentImageIndex = index;
        this.validateImageIndex();
        this.setCarouselPosition();
      },

      // Ensure carousel image index is not out of bounds.
      validateImageIndex() {
        const limit = this.imageArray.length - 1;
        if (this.currentImageIndex < 0) this.currentImageIndex = limit;
        if (this.currentImageIndex > limit) this.currentImageIndex = 0;
      },

      // Set carousel to correct position.
      setCarouselPosition() {
        const width = this.getCarouselWidth();
        const content = window.document.getElementById('carousel-content');
        const newLeftMargin = this.currentImageIndex * width;
        content.style.transform = `translate(-${newLeftMargin}px, 0)`;
      },

      /*
       * Utils
       *
       */

      // Get the width of the current carousel unit.
      getCarouselWidth() {
        const wrapper = window.document.getElementById('carousel-wrapper');
        const style = window.getComputedStyle(wrapper, null);
        return style.getPropertyValue('width').replace('px', '');
      },

      // Create and return a random int between floor and ceiling.
      randomInt(floor, ceiling) {
        const diff = ceiling - floor;
        return Math.floor(Math.random() * diff) + floor + 1;
      },
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

  // Style vars & resused elements
  $desktop-width: 75%;
  $black: #333;
  $inactive-white: rgba(255, 255, 255, 0.70);
  $hover-white: rgba(255, 255, 255, 0.90);
  $grid: 4px;
  $two-grid: $grid * 2;
  $three-grid: $grid * 2;
  $four-grid: $grid * 4;
  $six-grid: $grid * 6;

  .img-transition {
    -webkit-transition: ease-in-out all 0.5s;
    -moz-transition: ease-in-out all 0.5s;
    transition: ease-in-out all 0.5s;
  }

  // Carousel main styles.
  .carousel-wrapper {
    display: block;
    position: relative;
    width: $desktop-width;
    white-space: nowrap;
    border: $three-grid solid $black;
    overflow: hidden;
    margin: 0 auto;
    box-shadow: 0 $grid $two-grid rgba(0, 0, 0, 0.19);
  }

  .carousel-content {
    display: block;
    position: inherit;
    @extend .img-transition;

    img {
      display: inline-block;
      width: 100%;
      margin-bottom: -$two-grid;
    }
  }

  // Carousel navigation styles.
  .carousel-nav {
    display: block;
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    z-index: 1;

    .carousel-nav-left-arrow,
    .carousel-nav-right-arrow {
      display: inline-block;
      font-size: 12px;
      position: absolute;
      bottom: 47%;
      top: 48%;
      border-radius: $six-grid;
      width: $four-grid;
      height: $four-grid;
      padding: $grid;
      color: $black;
      background: $inactive-white;
      @extend .img-transition;
      &:hover {
        cursor: pointer;
        background: $hover-white;
      }
    }

    .carousel-nav-left-arrow {
      left: $two-grid;
    }

    .carousel-nav-right-arrow {
      right: $two-grid;
    }

    .nav-button-wrapper {
      display: block;
      position: absolute;
      bottom: $four-grid;
      left: 0;
      right: 0;
      text-align: center;
    }

    .nav-button {
      cursor: pointer;
      display: inline-block;
      height: $two-grid;
      width: $two-grid;
      background: $inactive-white;
      border: $grid solid rgba(255, 255, 255, 0.0);
      margin: 0 $two-grid;
      @extend .img-transition;
      &:hover {
        background: $hover-white;
      }

      &.active {
        border: $grid solid #252525;
      }
    }
  }
</style>
