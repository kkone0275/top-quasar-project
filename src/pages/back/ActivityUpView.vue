<template>
  <h5 class="text-center">揪團管理</h5>
  <!-- <q-btn class="add" style="background: #F3A308; color: white" @click="openAdd(-1)" label="新增揪團" /> -->
  <!-- <table class="box" style="width: 60%; " border="1">
          <thead>
            <tr align="left">
              <th>圖片</th>
              <th>名稱</th>
              <th>管理</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(product, idx) in products" :key="product._id">
              <td align="center">
                <img :src="product.image" :aspect-ratio="1" :width="100" :height="100"
                style="object-fit: cover; margin: auto;">
              </td>
              <td>{{ product.name }}</td>
              <td align="center">
                <q-btn color="primary" icon="edit" @click="openAdd(idx)" />
              </td>
            </tr>
          </tbody>
        </table> -->

        <div style="max-width: 100%">
        <div class="q-pa-md q-gutter-md">
          <q-item clickable v-ripple class="text-center">
            <q-item-section>商品名稱</q-item-section>
            <q-item-section>商品價格</q-item-section>
            <q-item-section>商品名稱</q-item-section>
            <q-item-section>上架狀態</q-item-section>
            <q-item-section>商品分類</q-item-section>
            <q-item-section>管理/編輯</q-item-section>
          </q-item>
          <q-separator />
          <!-- 下面的商品們 -->
          <q-item clickable v-ripple class="text-center" style="background: #fff;" v-for="(product, idx) in products" :key="product._id">
            <q-item-section>
              <img :src="product.image" :aspect-ratio="1" :width="150" :height="150"
                style="object-fit: cover; margin: auto;">
            </q-item-section>
            <q-item-section>{{ product.name }}</q-item-section>
            <q-item-section>{{ product.price }} 元</q-item-section>
            <q-item-section>
              <q-toggle :label="` ${product.sell ? '有' : '未'} 上架`" v-model="product.sell" checked-icon="check"
                color="green" unchecked-icon="clear" style="position: absolute;
          left: 54%;" />
            </q-item-section>
            <q-item-section>{{ product.category }}</q-item-section>
            <q-item-section>
              <div>
                <q-btn round color="secondary" size="20px" icon="save_as" @click="openAdd(idx)" />
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

            <q-form @submit="submit">
              <div class="flex row justify-between" style="padding: 16px 50px 16px 50px;">
              <q-input class="col-12" style="padding:10px ;" filled v-model="form.name" label="活動名稱" lazy-rules :rules="[rules.required]"/>
              <q-input class="col-12" style="padding:10px ;" filled v-model="form.math" label="參與人數" lazy-rules :rules="[rules.required,rules.price]"/>
              <q-input class="col-12" style="padding:10px ;" filled v-model="form.price" label="活動價格" lazy-rules :rules="[rules.required,rules.price]"/>
              <q-input class="col-12" style="padding: 10px;" filled v-model="form.description" label="商品說明"
              clearable type="textarea" @keydown="processTextareaFill"
              @focus="processTextareaFill"
              :rules="[rules.required]"/>
              <q-file class="col-12 ac-image" filled bottom-slots v-model="form.image" label="活動圖片" counter>
                <template v-slot:prepend>
                  <q-icon name="cloud_upload" @click.stop.prevent />
                </template>
                <template v-slot:append>
                  <q-icon name="close" @click.stop.prevent="model = null" class="cursor-pointer" />
                </template>

                <template v-slot:hint>
                  請上傳.jpg檔
                </template>
              </q-file>
              <q-file class="col-11" filled v-model="form.images" label="請選擇主圖片(可複選)" use-chips multiple>
                <template v-slot:prepend>
                  <q-icon name="attach_file"></q-icon>
                </template>
              </q-file>
                <div class="row" >
                    <q-img class="q-ml-lg" v-for="image in images" :key="image" :src="image" width="100px" />
                </div>

              <q-select class="col-8" filled :options="categories" v-model="form.category" label="活動地點" :rules="[rules.required]" />

              <q-select class="col-8" filled :options="genres" v-model="form.genre" label="活動類型" :rules="[rules.required]" />

              <q-checkbox class="col-8" style="margin-bottom: 1.2rem;" v-model="form.sell" label="上架" />

              <q-btn class="col-6" style="padding: 10px;" flat label="Cancel" color="primary" :disabled="form.loading" v-close-popup />

              <q-btn class="col-6" flat label="Add Send" type="submit" :disabled="form.loading" color="primary" />
              </div>
            </q-form>

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
</style>

<script setup>
import { apiAuth } from '../../boot/axios.js'
import { ref, reactive } from 'vue'
import Swal from 'sweetalert2'

const categories = ['台北市', '新北市', '新竹市', '台中市', '雲林縣', '台中市']
const genres = ['運動', '聊天', '吃飯', '散步', '唱歌', '活動', '露營', '旅行', '閱讀']
const rules = {
  required (value) {
    return !!value || '欄位必填'
  },
  price (value) {
    return value >= 0 || '價格錯誤'
  }
}

const products = reactive([])
const images = ref([])
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

// watch(() => form.images, (value) => {
//   images.value = []
//   value.forEach((img) => previewUrlHandler(img))
// })
// const previewUrlHandler = (file) => {
//   if (file && file.type.startsWith('image/')) {
//     const reader = new FileReader()
//     reader.addEventListener('load', (event) => {
//       images.value.push(event.target.result)
//     })
//     reader.readAsDataURL(file)
//   } else {
//     images.value.push(file)
//   }
// }

const submit = async () => {
  form.loading = true
  const fd = new FormData()
  fd.append('name', form.name)
  fd.append('math', form.math)
  fd.append('price', form.price)
  fd.append('description', form.description)
  fd.append('sell', form.sell)
  fd.append('image', form.image)
  fd.append('genre', form.genre)
  fd.append('category', form.category)
  for (const i of form.images) {
    fd.append('images', i)
  }

  try {
    if (form._id.length === 0) {
      const { data } = await apiAuth.post('/products', fd)
      products.push(data.result)
      Swal.fire({
        icon: 'success',
        title: '成功',
        text: '新增成功'
      })
    } else {
      const { data } = await apiAuth.patch('/products/' + form._id, fd)
      products[form.idx] = data.result
      Swal.fire({
        icon: 'success',
        title: '成功',
        text: '編輯成功'
      })
    }
    form.dialog = false
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: '失敗',
      text: error?.response?.data?.message || '發生錯誤'
    })
  }
  form.loading = false
}

(async () => {
  try {
    const { data } = await apiAuth.get('/products/all')
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
