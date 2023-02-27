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

        <p class="type">活動類型:</p>
        <q-card-section>
          <q-chip class="type_tag2" color="red-12" text-color="white">
          {{ genre }}
          </q-chip>
        </q-card-section>

        <p class="city">活動城市:</p>
        <q-card-section>
          <q-chip class="type_tag1" color="red-12" text-color="white">
          {{ category }}
          </q-chip>
        </q-card-section>
        <div class="text-subtitle2 week">活動日期: </div>
        <div class="text-subtitle2 date">{{ date.slice(0,10) }}</div>
        <div class="text-subtitle2 math">活動人數:  </div>
        <p class="people">{{ math }} 人</p>
        <div class="text-subtitle2 price">參加花費: $ {{ price }}</div>

      </q-card-section>

      <!-- <q-card-section class="des">
        {{ description }}
      </q-card-section> -->

      <q-card-section class="q-pt-none">

        <!-- <q-btn class="q-px-lg" color="pink-5" :to="'/products/' + _id" label="More" @click="opendis = true" /> -->
        <q-btn class="like" round color="deep-orange" icon="favorite" @click="editCart({_id, quantity: 1})" />

      </q-card-section>
    </q-card>
</template>

<script setup>
import { useUserStore } from 'src/stores/user'

const user = useUserStore()
const { editCart } = user
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
  math: {
    type: Number,
    default: 0
  },
  date: {
    type: String,
    default: ''
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
  genre: {
    type: String,
    default: ''
  },
  category: {
    type: String,
    default: ''
  }
})

</script>

<style scoped>
.q-card{
  width: 100%;
  height: 105%;
  border-radius: 10px;
  margin-top: -4rem;
  margin-bottom: 10px;
  margin-left: 16px;
}

.like{
  margin-left: 135px;
}

</style>

<style lang="scss" scoped>
.img{
  height: 320px;
  width: 360px;
  display: table-cell;
  vertical-align: middle;
  position: relative;
  margin-top: 0px;
  overflow: hidden;
  object-fit:scale-down;
}

.name{
  font-size: 27px;
  font-weight: bold;
  color: #8A8888;
  margin-left: 8px;
  margin-top: 0px;
}

.price{
  font-size: 28px;
  font-weight: bold;
  color: #8A8888;
  margin-left: 8px;
  margin-top: 0px;
  margin-bottom: 15px;
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
  margin-top: 40px;
  color: #8A8888;
}
.date{
  position: absolute;
  font-size: 24px;
  font-weight: bolder;
  margin-left: 8px;
  color: #1778D1;
  top:84px;
  left: 132px;
}

.week{
  position: absolute;
  font-size: 22px;
  font-weight: bold;
  margin-left: 8px;
  margin-top: -75px;
  color: #8A8888;
}

.type_tag1{
  height: 24px;
  width: 85px;
  position: relative;
  margin-top: 28px;
}

.type_tag2{
  width: 70px;
  height: 25px;
  position: absolute;
  margin-top: 6px;
}

.people{
  position:absolute;
  top: 142px;
  left: 140px;
  font-size: 22px;
  font-weight: bolder;
  color: #8A8888;
}

.math{
  position: relative;
  color: #8A8888;
  font-size: 22px;
  font-weight: bold;
  left: 8px;
  top:-10px
}

.type{
  position: absolute;
  font-size: 22px;
  font-weight: bold;
  margin-left: 8px;
  margin-top: 8px;
  color: #8A8888;
}
</style>
