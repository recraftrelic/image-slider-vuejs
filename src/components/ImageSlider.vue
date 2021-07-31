<template>
  <div class="mt-10">
    <div class="relative slider-content">
      <div
        v-for="(slide, i) in slides"
        v-bind:key="slide"
        class="absolute shadow-lg"
        v-bind:data-index="i"
      >
        <img v-bind:src="slide" />
      </div>
    </div>
    <div class="relative flex justify-between">
      <button class="mt-4 pl-5 pr-5 pt-3 pb-3 rounded text-white font-bold bg-green-400" v-on:click="onPrevious()">Previous</button>
      <button class="mt-4 pl-5 pr-5 pt-3 pb-3 rounded text-white font-bold bg-green-400" v-on:click="onNext()">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageSlider',
  data () {
    return {
      currentSlide: 0,
    }
  },
  props: {
    slides: Array
  },
  methods: {
    animate (elem, animation, onAnimationEnd) {
      const plainClassList = Array.prototype.slice.call(elem.classList)
      const animationsToRemove = plainClassList.filter(
        className => className.includes('animate__')
      );

      elem.classList.remove('hidden', ...animationsToRemove);
      elem.classList.add('animate__animated', animation);

      if (onAnimationEnd) {
        elem.addEventListener('animationend', onAnimationEnd, {once: true});
      }
    },
    getNextSlideIndex () {
      if (this.currentSlide + 1 < this.slides.length) {
        return this.currentSlide + 1;
      }

      return 0;
    },
    getPreviousSlideIndex () {
      if (this.currentSlide > 0) {
        return this.currentSlide - 1;
      }

      return this.slides.length - 1;
    },
    onNext () {
      const element = document.querySelector(`[data-index="${this.currentSlide}"]`);
      this.animate(element, 'animate__fadeOutLeft', () => {
        element.classList.add('hidden');
      })

      const nextSlideIndex = this.getNextSlideIndex();

      const nextElement = document.querySelector(`[data-index="${nextSlideIndex}"]`);
      this.animate(nextElement, 'animate__fadeInRight');

      this.currentSlide = nextSlideIndex;
    },
    onPrevious () {
      const element = document.querySelector(`[data-index="${this.currentSlide}"]`);
      this.animate(element, 'animate__fadeOutRight', () => {
        element.classList.add('hidden');
      })

      const previousSlideIndex = this.getPreviousSlideIndex();

      const previousElement = document.querySelector(`[data-index="${previousSlideIndex}"]`);
      this.animate(previousElement, 'animate__fadeInLeft');

      this.currentSlide = previousSlideIndex;
    }
  },
  mounted () {
    for (let i = 0; i < this.slides.length; i++) {
      if (i !== this.currentSlide) {
        const element = document.querySelector(`[data-index="${i}"]`);
        element.classList.add('hidden');
      }
    }
  }
}
</script>

<style scoped>
.slider-content {
  height: 400px;
}
</style>