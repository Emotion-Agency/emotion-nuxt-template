<template>
  <div
    ref="pages"
    style="overflow: hidden; width: 100%; height: 100vh; position: relative"
    @click="pagesAnim"
  >
    <div class="page page-1">
      <div class="content">
        <h1 ref="h1" @click="h1Anim">
          At the <span class="italic">intersection</span> of technology,
          marketing and aesthetics
        </h1>
        <div class="overlay"></div>
      </div>
    </div>
    <div class="page page-2">
      <div class="content">
        <h1 ref="h12">We are Emotion, a creative It Team</h1>
      </div>
    </div>
  </div>
</template>

<script>
import gsap from 'gsap'

export default {
  async mounted() {
    const h1 = this.$refs.h1
    const h12 = this.$refs.h12
    const { default: splitting } = await import('splitting')
    splitting({ target: h1, by: 'chars' })
    splitting({ target: h12, by: 'chars' })

    h1.style.opacity = 1
    h12.style.opacity = 1

    setTimeout(() => {
      this.h1Anim(h1)
    }, 500)
  },
  methods: {
    h1Anim(h1) {
      const spans = [...h1.querySelectorAll('.char')]

      const baseLength = 52
      const coef = baseLength / spans.length

      const stagger = coef * 0.015
      const duration = coef * 1

      gsap.defaults({ overwrite: 'auto' })
      const tl = gsap.timeline()

      // tl.set(spans, {
      //   opacity: 0,
      //   transform: 'translateY(100%) rotateX(-90deg) scale(0.9)',
      // })

      tl.to(spans, {
        duration,
        rotateX: 0,
        scale: 1,
        y: 0,
        opacity: 1,
        ease: 'expo.out', // back.out(1.05)
        stagger,
      })
    },
    pagesAnim() {
      const pages = this.$refs.pages
      const page1 = pages.querySelector('.page-1')
      const page2 = pages.querySelector('.page-2')

      const content1 = page1.querySelector('.content')
      const content2 = page2.querySelector('.content')

      const overlay1 = content1.querySelector('.overlay')

      const h12 = content2.querySelector('h1')
      setTimeout(() => {
        this.h1Anim(h12)
      }, 300)

      const tl = gsap.timeline()

      tl.to(page1, {
        duration: 2,
        x: '-100%',
        ease: 'power4.out',
      })
      tl.to(
        overlay1,
        {
          duration: 1.6,
          opacity: 1,
          ease: 'expo.out',
        },
        0
      )
      tl.to(
        content1,
        {
          duration: 2,
          x: '80%',
          ease: 'power4.out',
        },
        0
      )
      tl.to(
        page2,
        {
          duration: 0.2,
          opacity: 1,
          ease: 'power4.out',
        },
        0
      )
      tl.to(
        page2,
        {
          duration: 2,
          x: '0%',
          skewX: 0,
          ease: 'power4.out',
        },
        0
      )
      tl.from(
        content2,
        {
          duration: 2,
          x: '-80%',
          skewX: 5,
          ease: 'power4.out',
        },
        0
      )
    },
  },
}
</script>

<style lang="scss">
body {
  background-color: var(--black);
}
.italic {
  font-style: italic;
  font-weight: 400;
}
h1 {
  position: absolute;
  width: 1205px;
  // height: 327px;
  opacity: 0;
  left: 85px;
  // top: 58%;
  bottom: 9%;
  z-index: 2;
  font-family: HelveticaNeueCyr;
  font-style: normal;
  font-weight: 300;
  font-size: 95px;
  line-height: 115%;
  letter-spacing: -0.005em;
  text-transform: uppercase;
  color: #d6d6d6;
  perspective: 2000px;
}
.char {
  display: inline-block;
  transform-origin: top;
  backface-visibility: hidden;
  opacity: 0;
  transform: translateY(100%) rotateX(-90deg) scale(0.9);
  letter-spacing: -0.005em;
}

.word {
  display: inline-block;
}

.page {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  overflow: hidden;
  right: 0;
  background-color: var(--black);
}

.page-1 {
  z-index: 1;
}

.page-2 {
  transform: translateX(97%) skewX(5deg);
  z-index: 10;
  opacity: 0;
}

.overlay {
  z-index: 7;
  background-color: #d6d6d6;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: 0;
}

.content {
  width: 100%;
  height: 100%;
  position: relative;
}
</style>
