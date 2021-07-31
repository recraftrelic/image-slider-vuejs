<template>
  <div class="mt-10">
    <div class="relative slider-content">
      <div
        v-for="(slide, i) in slides"
        v-bind:key="slide"
        v-bind:data-index="i"
        class="absolute shadow-lg"
      >
        <img v-bind:src="slide" />
      </div>
    </div>
    <div class="flex justify-between">
      <button
        class="pt-3 pb-3 pl-5 pr-5 bg-green-400 rounded font-bold text-white"
        v-on:click="onPrevious()"
      >
        Previous
      </button>
      <button
        class="pt-3 pb-3 pl-5 pr-5 bg-green-400 rounded font-bold text-white"
        v-on:click="onNext()"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageSlider',
  data () {
    return {
      currentSlideIndex: 0,
    }
  },
  props: {
    slides: Array
  },
  methods: {
    animate (element, animation, onAnimationEnd) {
      const plainClassList = Array.prototype.slice.call(element.classList);
      const animationsToRemove = plainClassList.filter(
        className => className.includes('animate__')
      )

      element.classList.remove('hidden', ...animationsToRemove);
      element.classList.add('animate__animated', animation);

      if (onAnimationEnd) {
        element.addEventListener('animationend', onAnimationEnd, {once:true})
      }
    },
    getNextSlideIndex () {
      if (this.currentSlideIndex + 1 < this.slides.length) {
        return this.currentSlideIndex + 1;
      }

      return 0;
    },
    getPreviousSlideIndex () {
      if (this.currentSlideIndex > 0) {
        return this.currentSlideIndex - 1;
      }

      return this.slides.length - 1;
    },
    onNext () {
      const element = document.querySelector(`[data-index="${this.currentSlideIndex}"]`)
      this.animate(element, 'animate__fadeOutLeft', () => {
        element.classList.add('hidden')
      });

      const nextSlideIndex = this.getNextSlideIndex();

      const nextElement = document.querySelector(`[data-index="${nextSlideIndex}"]`)
      this.animate(nextElement, 'animate__fadeInRight');

      this.currentSlideIndex = nextSlideIndex;
    },
    onPrevious () {
      const element = document.querySelector(`[data-index="${this.currentSlideIndex}"]`)
      this.animate(element, 'animate__fadeOutRight', () => {
        element.classList.add('hidden')
      });

      const previousSlideIndex = this.getPreviousSlideIndex();

      const previousElement = document.querySelector(`[data-index="${previousSlideIndex}"]`)
      this.animate(previousElement, 'animate__fadeInLeft');

      this.currentSlideIndex = previousSlideIndex;
    }
  },
  mounted () {
    this.slides.forEach((_, index) => {
      if (index !== this.currentSlideIndex) {
        const element = document.querySelector(`[data-index="${index}"`);
        element.classList.add('hidden');
      }
    })
  }
}
</script>

<style scoped>
.slider-content {
  height: 250px;
}
</style>
