<template>
  <div class="page">
    <img class="main-img" :src="product.image">
    <p class="ac-name">活動名稱: {{ product.name }}</p>
    <p>參與活動所需費用: ${{ product.price }}</p>
    <p style="white-space: pre;">{{ product.description }}</p>
    <q-form v-model="valid" @submit="addEvent">
      <q-input v-model.number="quantity" type="number" label="參與人數" />
      <q-btn label="參與活動" type="submit" color="primary" />
    </q-form>
  </div>
  <div class="right-img">
    <h5>活動介紹圖片</h5>
    <img class="side-img" :src="product.images[0]">
    <img class="side-img" :src="product.images[1]">
    <img class="side-img" :src="product.images[2]">
    </div>
</template>

<script setup>
import { api, apiAuth } from '../../boot/axios'
import { reactive } from 'vue'
import { useRoute } from 'vue-router'
import { Swal } from 'sweetalert2'
// import { useUserStore } from 'src/stores/user'

// const user = useUserStore()
const route = useRoute()
// const { addMember } = user

const product = reactive({
  _id: '',
  name: '',
  price: 0,
  description: '',
  image: '',
  sell: true,
  category: ''
});

(async () => {
  try {
    const { data } = await api.get('/products/' + route.params.id)
    product._id = data.result._id
    product.name = data.result.name
    product.price = data.result.price
    product.description = data.result.description
    product.image = data.result.image
    product.images = data.result.images
    product.sell = data.result.sell
    product.category = data.result.category
    console.log(product)
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: '失敗',
      text: '取得商品失敗'
    })
  }
})()

const addEvent = async () => {
  try {
    const { data } = await apiAuth.post('/orders', product)
    console.log(data)
    Swal.fire({
      icon: 'success',
      title: '成功',
      text: '送出成功'
    })
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: '失敗',
      text: error?.response?.data?.message || '發生錯誤'
    })
  }
}
</script>

<style scoped>
  .page{
    width: 100vh;
    position: absolute;
    text-align: left;
    margin-top: 2%;
    left: 5%;
  }

  .ac-name{
    font-size: 25px;
    font-weight: 600;
    margin-top: 10px;
  }

  .main-img{
    width: 50%;
  }

  .right-img{
    position: absolute;
    right: 20%;
  }

  .side-img{
    width: 250px;
  }
</style>
