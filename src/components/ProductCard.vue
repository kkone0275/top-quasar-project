<template>
  <q-card class="my-card">
     <!-- 圖片 -->
      <div>
        <router-link class="col-12 col-sm-5 row justify-center items-center" :to="'/products/' + _id">
          <img class="img" :src="image">
        </router-link>
      </div>

      <!-- <img :src="image"> -->

      <q-card-section>
        <!-- <q-item-label>
          <router-link :to="'/products/' + _id">{{ name }}</router-link>
        </q-item-label> -->
        <div class="text-subtitle2 name">{{ name }}</div>

        <p class="city">活動城市:</p>
        <q-card-section>
          <q-chip class="type_tag" color="red-12" text-color="white">
          {{ category }}
          </q-chip>
        </q-card-section>

        <div class="text-subtitle2 price">參加花費: ${{ price }}</div>
      </q-card-section>

      <!-- <q-card-section class="des">
        {{ description }}
      </q-card-section> -->

      <q-card-section class="q-pt-none">

        <!-- <q-btn class="q-px-lg" color="pink-5" :to="'/products/' + _id" label="More" @click="opendis = true" /> -->
        <!-- <q-btn class="like" round color="deep-orange" icon="favorite" @click="editCart({_id, quantity: 1})" /> -->

      </q-card-section>
    </q-card>
</template>

<script setup>

defineProps({
  _id: {
    type: String,
    default: ''
  },
  name: {
    type: String,
    default: ''
  },
  price: {
    type: Number,
    default: 0
  },
  description: {
    type: String,
    default: ''
  },
  image: {
    type: String,
    default: ''
  },
  sell: {
    type: Boolean,
    default: false
  },
  category: {
    type: String,
    default: ''
  }
})

</script>

<style>
.q-card{
  width: 380px;
  height: 560px;
  border-radius: 10px;
  margin-top: -4.5rem;
  margin-bottom: 20px;
}

.like{
  margin-left: 135px;
}

/* .des{
  margin: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  color: gray;
} */
</style>

<style lang="scss" scoped>
.img{
  position: relative;
  height: 252px;
  display: table-cell;
  vertical-align: middle;
  position: relative;
  margin-top: 12px;
}

.name{
  font-size: 27px;
  font-weight: bold;
  color: #8A8888;
  margin-left: 8px;
  margin-top: 12px;
}

.price{
  font-size: 28px;
  font-weight: bold;
  color: #8A8888;
  margin-left: 8px;
  margin-top: 12px;
}

.q-chip{
  width: 88px;
  height: 32px;
  font-size: 18px;
  position: relative;
  top: 18%;
  left: 34%;
}
.city{
  position: absolute;
  font-size: 24px;
  font-weight: bold;
  margin-left: 8px;
  color: #8A8888;
}
</style>
