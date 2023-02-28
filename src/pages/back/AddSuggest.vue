<template>
  <h5 class="text-center">建議蒐集管理</h5>
        <div style="max-width: 100%">
        <div class="q-pa-md q-gutter-md">
          <q-item clickable v-ripple class="text-center">
            <q-item-section>邀請人</q-item-section>
            <q-item-section>敘述圖片</q-item-section>
            <q-item-section>空閒建議</q-item-section>
            <q-item-section>集合地點</q-item-section>
            <q-item-section>提議敘述</q-item-section>
            <q-item-section>接受/取消</q-item-section>
          </q-item>
          <q-separator />
          <!-- 下面的商品們 -->
          <q-item clickable v-ripple class="text-center" style="background: #fff;" v-for="(product, idx) in products"
            :key="product._id">
            <q-item-section>
              <img src='https://source.boringavatars.com/beam/256/{{user.name}}colors=ffabab,ffdaab,ddffab,abe4ff,d9abff' style="height: 80px">
            </q-item-section>
            <q-item-section>
              <img :src="product.oneimage" :aspect-ratio="1" :width="150" :height="150"
                style="object-fit: cover; margin: auto;">
            </q-item-section>
            <q-item-section>{{ product.me }}</q-item-section>
            <q-item-section>{{ product.land }}</q-item-section>
            <!-- <q-item-section>
              <q-toggle :label="` ${product.sell ? '有' : '未'} 上架`" v-model="product.sell" checked-icon="check"
                color="green" unchecked-icon="clear" style="position: absolute; left: 54%;" />
            </q-item-section> -->
            <q-item-section>{{ product.detailed }}</q-item-section>
            <q-item-section>
              <div>
                 <q-btn no-caps color="amber-8" class="btn-left" label="接 受" @click="openAdd(idx)"/>
                 <q-btn no-caps color="red-5" label="取 消" @click="openAdd(idx)"/>
              </div>
            </q-item-section>
          </q-item>
        </div>
      </div>

  <div class="q-pa-md">
    <div class="row justify-center">
      <div class="col-12 col-md-1">
        <q-dialog v-model="form.dialog" persistent>
          <q-card style="max-width: 800px ">
            <div class="text-h6" align="center">{{ form._id.length > 0 ? '編輯揪團' : '新增揪團' }}</div>
          </q-card>
        </q-dialog>
    </div>
  </div>
</div>
</template>

<style scoped>
.add{
  position: absolute;
  right: 16%;
}

.q-card{
  height: 600px;
}

.text-h6{
  margin-top: 20px;
}

.q-file{
  margin-top: 25px;
  margin-bottom: 20px;
}

.q-select{
  margin-top: 25px
}

.box{
  position: absolute;
  top: 30%;
  left: 25%;
}
.ac-image{
  object-fit: scale-down;
}

.btn-left{
  margin-right: 20px;
}
</style>

<script setup>
import { apiAuth } from '../../boot/axios.js'
import { reactive } from 'vue'
import Swal from 'sweetalert2'

const products = reactive([])
const form = reactive({
  _id: '',
  name: '',
  math: 0,
  price: 0,
  description: '',
  image: undefined,
  images: [],
  sell: false,
  genre: '',
  category: '',
  valid: false,
  loading: false,
  dialog: false,
  idx: -1
})

const openAdd = (idx) => {
  if (idx === -1) {
    form._id = ''
    form.name = ''
    form.math = 0
    form.price = 0
    form.description = ''
    form.image = undefined
    form.images = []
    form.sell = false
    form.genre = ''
    form.category = ''
    form.valid = false
    form.loading = false
    form.idx = -1
  } else {
    form._id = products[idx]._id
    form.name = products[idx].name
    form.math = products[idx].math
    form.price = products[idx].price
    form.description = products[idx].description
    form.image = products[idx].image
    form.images = products[idx].images
    form.sell = products[idx].sell
    form.genre = products[idx].genre
    form.category = products[idx].category
    form.valid = false
    form.loading = false
    form.idx = idx
  }
  form.dialog = true
}

(async () => {
  try {
    const { data } = await apiAuth.get('/invites/all')
    products.push(...data.result)
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: '失敗',
      text: error?.response?.data?.message || '發生錯誤'
    })
  }
})()
</script>
