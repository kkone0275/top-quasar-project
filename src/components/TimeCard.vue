<template>
  <q-card class="my-card">
      <!-- <img :src="image"> -->

      <q-card-section>

        <img class="avatar" :src="`https://source.boringavatars.com/beam/256/$%7B${name}colors=ffabab,ffdaab,ddffab,abe4ff,d9abff`">
        <div class="text-subtitle2 name">{{ name }}</div>

        <p class="wordpage">空閒敘述:</p>
      <q-card-section class="q-pt-none about">
        {{ description }}
      </q-card-section>
      </q-card-section>

      <div class="text-subtitle2 date"><q-icon name="schedule" />{{ date }}</div>
      <div class="text-subtitle2 hour">0{{ hour }}小時</div>

      <q-card-section>
        <div class="chip01">
          <q-chip class="type_tag q-chip-out" color="red-12" text-color="white">
          {{ category }}
          </q-chip>
        </div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <q-btn class="q-px-lg message" unelevated rounded color="teal-6" label="時  間  填  空" @click="prompt = true" />

        <!-- 展開選單 -->
      <q-dialog v-model="prompt" persistent>
          <q-card style="min-width: 600px;height: 700px;">
            <q-card-section class="flex">
              <img class="avatar-in" :src="`https://source.boringavatars.com/beam/256/$%7B${name}colors=ffabab,ffdaab,ddffab,abe4ff,d9abff`">
              <div class="text-subtitle2 name-in">{{ name }}</div>
              <q-btn flat class="close-btn" round dense icon="close" v-close-popup />
              <div class="date-in"><q-icon name="schedule" />{{ date }}</div>
              <div class="hour-in">0{{ hour }}小時</div>
            </q-card-section>

            <q-card-section>
              <q-chip class="type_in" color="red-12" text-color="white">
                {{ category }}
              </q-chip>
            </q-card-section>

            <q-card-section class="text-in">
              <p>空閒敘述:</p>
              <div>{{ description }}</div>
            </q-card-section>

            <q-card-section class="q-pt-none">
              <q-input class="dating" outlined v-model="me" label="我想請你，跟我一起:" autofocus @keyup.enter="blank = false" />
            </q-card-section>

            <q-card-section class="q-pt-none">
              <q-input class="dating" outlined v-model="land" label="集合地點:" autofocus @keyup.enter="blank = false" />
            </q-card-section>

            <q-card-section class="q-pt-none">
              <q-input class="dating" outlined v-model="detailed" label="提議詳細描述:" autofocus @keyup.enter="blank = false" />
            </q-card-section>

            <q-card-section class="q-pt-none">
              <q-input class="dating" outlined v-model="detailed" label="請上傳圖片:" autofocus @keyup.enter="blank = false" />
            </q-card-section>

              <q-btn class="q-px-lg message2" unelevated rounded color="teal-6" label="時  間  填  空"  />
          </q-card>
      </q-dialog>

      <!-- <q-file class="dating" filled bottom-slots v-model="dating" label="圖片補充" counter>
        <template v-slot:prepend>
          <q-icon name="cloud_upload" @click.stop.prevent />
        </template>
        <template v-slot:append>
          <q-icon name="close" @click.stop.prevent="model = null" class="cursor-pointer" />
        </template>
      </q-file>

      <q-card-actions align="right" class="text-primary">
        <q-btn flat label="Cancel" v-close-popup />
        <q-btn class="q-px-lg message" unelevated rounded color="teal-6" label="時  間  填  空"  />
      </q-card-actions>
    </q-card>
    </q-dialog> -->

      </q-card-section>
    </q-card>
</template>

<script setup>
import { ref } from 'vue'

const prompt = ref(false)

defineProps({
  _id: {
    type: String,
    default: ''
  },
  name: {
    type: String,
    default: ''
  },
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
  width: 380px;
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

.avatar-in{
  width: 120px;
  margin-left: 2%;
  margin-top: 20px;
}

.name{
  position: absolute;
  font-size: 30px;
  font-weight: bold;
  color: #686868;
  top: 38px;
  left: 128px;
}

.name-in{
  position: relative;
  font-size: 42px;
  font-weight: bold;
  color: #686868;
  top: 40px;
  left: 34px;
}

.message{
  width: 100%;
  font-size: 16px;
  font-weight: bold;
  margin-top: 65px;
}

.message2{
  width: 82%;
  font-size: 16px;
  font-weight: bold;
  margin-top: 18px;
  margin-left: 9%;
}

.about{
  position: absolute;
  margin-top: 30px;
  font-weight: bold;
  color: #8f8f8f;
  font-size: 18px;
}

.q-chip-out{
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

.type_in{
  position: absolute;
  width: 92px;
  height: 30px;
  font-size: 20px;
  top: -50px;
  left: 190px;
}

.dating{
  width: 500px;
  margin-left: 35px;
}

.close-btn{
  position: absolute;
  right: 2%;
}

.text-in{
  position: relative;
  top: -25px;
  font-size: 18px;
  margin-left: 32px;
  font-weight: bolder;
  color: #828282;
}
.date-in{
  position: absolute;
  font-size: 18px;
  font-weight: bolder;
  color: #828282;
  top: 128px;
  right: 130px;
}

.hour-in{
  position: absolute;
  font-size: 18px;
  font-weight: bolder;
  color: #828282;
  top: 128px;
  right: 60px;
}
</style>
