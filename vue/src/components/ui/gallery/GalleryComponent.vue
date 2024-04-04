<template>
  <div>
    <MainGalleryPhoto :photo="photos[selected]"/>
    <div class="button">
      <button class="button-arrow" @click="() => change(-1)">
        <i class="el-icon-arrow-left"></i>
      </button>
    </div>
    <div class="carousel">
      <div class="inner" ref="inner" :style="innerStyles">
        <div class="card" v-for="(photo, index) in photos" :key="index">
          <img 
            :class="selected == index ? 'selected' : ''"
            :src="photo.link"
          />
        </div>
      </div>
    </div>
    <div class="button">
      <button class="button-arrow" @click="() => change(1)">
        <i class="el-icon-arrow-right"></i>
      </button>
    </div>
  </div>
</template>
  
<script>
import MainGalleryPhoto from './MainGalleryPhoto.vue'

export default {
  name: 'GalleryComponent',
  components: {
    MainGalleryPhoto
  },
  props: {
    photos: [
      {
        link: {
          type: String,
          default: ''
        },
        headLine: {
          type: String,
          default: ''
        },
        description: {
          type: String,
          default: ''
        },
        id: {
          type: String,
          default: ''
        }
      }
    ]
  },
  data () {
    return {
      cards: this.photos,
      selected: 0,
      transitioning: false,
      innerStyles: {},
      step: ''
    }
  },

  mounted () {
    this.selected = Math.round(this.photos.length / 2) - 2
    this.setStep()
    this.resetTranslate()
  },

  methods: {
    setStep () {
      const innerWidth = this.$refs.inner.scrollWidth
      const totalCards = this.cards.length
      this.step = `${innerWidth / totalCards}px`
    },

    change (change) {
      if (this.transitioning) return
      this.transitioning = true
      if (change < 0) {
        this.moveRight()
        this.afterTransition(() => {
          const card = this.cards.pop()
          this.cards.unshift(card)
          this.resetTranslate()
          this.transitioning = false
        })
      } else {
        this.moveLeft()
        this.afterTransition(() => {
          const card = this.cards.shift()
          this.cards.push(card)
          this.resetTranslate()
          this.transitioning = false
        })
      }
    },

    moveLeft () {
      this.innerStyles = {
        transform: `translateX(-${this.step})
          translateX(-${this.step})`
      }
    },

    moveRight () {
      this.innerStyles = {
        transform: `translateX(${this.step})
          translateX(${this.step})`
      }
    },

    afterTransition (callback) {
      const listener = () => {
        callback()
        this.$refs.inner.removeEventListener('transitionend', listener)
      }
      this.$refs.inner.addEventListener('transitionend', listener)
    },

    resetTranslate () {
      this.innerStyles = {
        transition: 'none',
      }
    }
  }
}
</script>

<style scoped lang="less">
.button {
  float: left;
  margin-left: 10;
  width: 50px;
  height: 50px;
  margin-top: 50px;
}

.button-arrow {
  align-items: center;
  justify-content: center;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: #ccc;
  border: none;
  cursor: pointer;
}

.carousel {
  float: left;
  margin-right: 10;
  justify-content: center;
  margin-top: 20px;
  width: 800px;
  height: 100px;
  overflow: hidden;
}

.inner {
  transition: transform 0.5s;
  white-space: nowrap;
  height: 120px;
}

.card {
  height: 100px;
  margin-right: 10px;
  display: inline-flex;
}

.selected {
  height: 120px;
}
</style>
