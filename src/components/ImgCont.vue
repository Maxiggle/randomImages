<template>
  <div class="img-cont">
    <img v-if="clicked" :src="img.urls.regular" :alt="img.alt_description">
    <span>{{ img.description }}</span>
  </div>
  <button @click="getImg()">Get a lucky picture</button>
</template>

<script>
import { ref } from '@vue/reactivity';
export default {
  setup(){
    const clicked = ref(false);
    const img = ref({})

    const getImgData = async ()=>{
      clicked.value = true;

      const API_URL = `/.netlify/functions/getImg`;
      let imgData;

      try {
        const res = await fetch(API_URL)
        imgData = await res.json();

      } catch (err) {
        console.log(err);
      }

      return imgData.data
    }

    const getImg = async ()=>{
      img.value = await getImgData()
    }

    return {getImg, img, clicked}
  },

}
</script>

<style scoped>
.img-cont{
  width: 100%;
  max-width: 540px;
  margin: 1rem 0;
}
.img-cont img{
  width: 100%;
  height: 100%;
  border-radius: 1rem;
  object-fit: cover;
  object-position: center ;
}

button{
  background: rgb(50, 13, 153);
  padding: 0.5rem 1rem;
  color: #fff;
  cursor: pointer;
  border: none;
  border-radius: 0.25rem;
}
</style>