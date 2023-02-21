<template>
  <q-card class="my-card">
      <!-- <img :src="image"> -->

      <q-card-section>
        <!-- <q-item-label>
          <router-link :to="'/products/' + _id">{{ name }}</router-link>
        </q-item-label> -->
        <img class="avatar" :src="`https://source.boringavatars.com/beam/256/$%7B${name}colors=ffabab,ffdaab,ddffab,abe4ff,d9abff`">
        <div class="text-subtitle2 name">{{ name }}</div>
        <!-- <div class="text-subtitle2">${{ price }}</div> -->
        <p class="wordpage">空閒敘述:</p>
      <q-card-section class="q-pt-none about">
        {{ description }}
      </q-card-section>
      </q-card-section>

      <div class="text-subtitle2 date"><q-icon name="schedule" />{{ date }}</div>
      <div class="text-subtitle2 hour">0{{ hour }}小時</div>

      <q-card-section>
        <div class="chip01">
          <q-chip class="type_tag" color="orange-14" text-color="white">
          {{ category }}
          </q-chip>
        </div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <q-btn class="q-px-lg time" unelevated rounded color="teal-6" :to="'/times/' + _id" label="時  間  填  空" @click="opendis = true" />
        <!-- <q-btn class="q-px-lg time" color="pink-5" :to="'/times/' + _id" label="時  間  填  空" @click="opendis = true" /> -->
        <!-- <q-btn class="like" round color="deep-orange" icon="favorite" @click="editCart({_id, quantity: 1})" /> -->
      </q-card-section>
    </q-card>
</template>

<script setup>
// import { useUserStore } from 'src/stores/user'
// const user = useUserStore()
// const { editCart } = user

defineProps({
  _id: {
    type: String,
    default: ''
  },
  name: {
    type: String,
    default: ''
  },
  // price: {
  //   type: Number,
  //   default: 0
  // },
  date: {
    type: String,
    default: ''
  },
  hour: {
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
  category: {
    type: String,
    default: ''
  }
})
</script>

<style scoped>
.q-card{
  width: 400px;
  height: 270px;
  margin-top: -3rem;
}

.like{
  margin-left: 130px;
}

.avatar{
  width: 80px;
  margin-left: 10px;
}

.name{
  position: absolute;
  font-size: 36px;
  font-weight: bold;
  color: #686868;
  top: 38px;
  left: 135px;
}

.time{
  width: 100%;
  font-size: 16px;
  font-weight: bold;
  margin-top: 65px;
}

.about{
  position: absolute;
  margin-top: 30px;
  font-weight: bold;
  color: #8f8f8f;
  font-size: 18px;
}

.q-chip{
  width: 88px;
  height: 30px;
  font-size: 18px;
  position: absolute;
  top: -140%;
  left: 32%;
}

.date{
  margin-top: 17%;
  margin-left: 30px;
  font-size: 18px;
  position: absolute;
  color: #8A8888;
}

.hour{
  margin-top: 17%;
  margin-left: 78%;
  font-size: 18px;
  font-weight: bolder;
  position: absolute;
  color: #8A8888;
}

.q-icon{
  margin-right: 6px;
  width: 10px;
}

.wordpage{
  position: absolute;
  margin-top: 8px;
  margin-left: 16px;
  font-size: 15px;
  color: #acacac;
  font-weight: 800;
}
</style>
