<template>
  <div class="main-photo">
    <div class="main-photo__image">
      <img :src="photo.link"/>
      <div class="main-photo__headLine">
        {{ photo.headLine }}
      </div>
      <div class="main-photo__description">
        {{ photo.description }}
      </div>
    </div>
  </div>
</template>
  
<script>
export default {
  name: 'MainGalleryPhoto',
  props: {
    photo: {
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
  }
}
</script>

<style scoped lang="less">
.main-photo {
  &__image {
    position: relative;
    display: flex;
    flex: 0 0 auto;
    justify-content: center;
    height: 600px;
    padding: 20px;
    margin-left: auto;
    margin-right: auto;
  }

  &__headLine {
    text-align: center;
    position: absolute;
    color: rgb(255, 255, 255);
    font-size: 60px;
    top: 520px;
  }

  &__description {
    text-align: center;
    position: absolute;
    color: rgb(255, 255, 255);
    font-size: 30px;
    top: 580px;
  }
}
</style>
