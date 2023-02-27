<template>
  <div class="page q-pa-md">
    <img class="main-img" :src="product.image">
    <p class="ac-name">活動名稱: <nav class="name">{{ product.name }}</nav></p>
    <p class="bc-name">活動標籤:</p>
    <p class="bc-name">活動時間:</p>
    <p class="date">{{ product.date }}</p>
    <p class="bc-name">活動人數:</p>
    <p class="math">{{ product.math }}  人</p>
    <q-chip class="type_tag q-chip-out" color="red-12" text-color="white">
    {{ product.genre }}
    </q-chip>
    <p class="bc-name">參與活動費用:  $ {{ product.price }}</p>
    <p class="bc-name">活動地點:  $ {{ product.price }}</p>
  </div>

    <div class="right-item">
      <h5 class="introduce">活動介紹圖片</h5>
      <div class="right-img">
        <img class="side-img" :src="product.images[0]">
        <img class="side-img" :src="product.images[1]">
        <img class="side-img" :src="product.images[2]">
      </div>
      <q-card class="my-card">
      <q-card-section>
        {{ product.description }}
      </q-card-section>
      </q-card>
      <q-form v-model="valid" @submit="addEvent">
      <!-- <q-input v-model.number="quantity" type="number" label="參與人數" /> -->
      <q-btn class="add-btn" label="參 與 活 動" type="submit" />
      </q-form>
    </div>
</template>

<script setup>
import { api, apiAuth } from '../../boot/axios'
import { reactive } from 'vue'
import { useRoute } from 'vue-router'
import Swal from 'sweetalert2'
// import { useUserStore } from 'src/stores/user'

const route = useRoute()
// const user = useUserStore()
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
    product.genre = data.result.genre
    product.category = data.result.category
    product.date = data.result.date
    product.math = data.result.math
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
      text: '活動參與成功'
    })
  } catch (error) {
    console.log(error)
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
    margin-top: 0.5%;
    left: 6%;
  }

  .name{
    position: absolute;
    top: 62.2%;
    left: 15%;
    font-size: 32px;
  }

  .ac-name{
    font-size: 25px;
    font-weight: 600;
    margin-top: 10px;
  }

  .bc-name{
    font-size: 25px;
    font-weight: 600;
    margin-top: 4px;
    margin-bottom: 12px;
  }

  .date{
    position: absolute;
    top: 74.6%;
    left: 15%;
    font-size: 26px;
    color: #1778D1;
    font-weight: bold;
  }

  .math{
    position: absolute;
    top: 80.5%;
    left: 15%;
    font-size: 26px;
    font-weight: bold;
  }

  .main-img{
    height: 500px;
    width: 800px;
    /* border: 2px solid rgb(251, 221, 26); */
    background-color: rgb(240, 239, 233);
    border-radius:6px;
    object-fit:scale-down;
    padding: 0px;
  }

  .right-img{
    margin-top: 2%;
    right: 10%;
  }

  .side-img{
    width: 250px;
  }
  .q-chip{
  width: 75px;
  height: 32px;
  font-size: 20px;
  position: absolute;
  top: 68.7%;
  left: 15%;
}

.right-item{
 position: absolute;
 width: 42%;
 right: 6%;
}
.introduce{
  font-weight: bold;
}

.add-btn{
  width: 40%;
  height: 20%;
  font-size: 26px;
  font-weight: bold;
  border-radius:10px;
  margin-top: 22px;
  margin-left: 28%;
  color: white;
  background-color: #FF775C;
}
</style>
