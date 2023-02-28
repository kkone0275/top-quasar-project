<template>
  <h5 class="text-center">揪團上架</h5>
  <q-btn class="add" style="background: #FFB300 ; color: white" @click="openAdd(-1)" label="新增揪團" />

  <div class="q-pa-md q-gutter-sm col-12" align="center">
    <q-btn v-for="gang,index in categories" unelevated rounded color="amber-7" :key="index" :label="gang" @click="gangActive=gang" />
  </div>

  <div style="max-width: 100%">
        <div class="q-pa-md q-gutter-md">
          <q-item clickable v-ripple class="text-center">
            <q-item-section>揪團圖片</q-item-section>
            <q-item-section>揪團名稱</q-item-section>
            <q-item-section>參與人數</q-item-section>
            <!-- <q-item-section>參加費用</q-item-section> -->
            <q-item-section>上架狀態</q-item-section>
            <q-item-section>揪團地區</q-item-section>
            <q-item-section>管理/編輯</q-item-section>
          </q-item>
          <q-separator />
          <!-- 下面的商品們 -->
          <q-item clickable v-ripple class="text-center" style="background: #fff;" v-for="(product, idx) in ac"
            :key="product._id">
            <q-item-section>
              <img :src="product.image" :aspect-ratio="1" :width="150" :height="150"
                style="object-fit: cover; margin: auto;">
            </q-item-section>
            <q-item-section>{{ product.name }}</q-item-section>
            <q-item-section>{{ product.math }} 人</q-item-section>
            <!-- <q-item-section>{{ product.price }} 元</q-item-section> -->
            <q-item-section>
              <q-toggle :label="` ${product.sell ? '有' : '未'} 上架`" v-model="product.sell" checked-icon="check"
                color="green" unchecked-icon="clear" style="position: absolute; left: 54%;" />
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

    <div class="row justify-center">
        <q-dialog v-model="form.dialog" persistent>
          <q-card class="sumbitcard">
            <div class="text-h6" align="center">{{ form._id.length > 0 ? '編輯揪團' : '新增揪團' }}</div>

            <q-form @submit="submit">
              <div class="flex row justify-between">
              <q-input class="col-12" filled v-model="form.name" label="活動名稱" lazy-rules :rules="[rules.required]"/>
              <q-input class="col-12" filled v-model="form.math" label="參與人數" lazy-rules :rules="[rules.required,rules.price]"/>
              <q-input class="col-12" filled v-model="form.price" label="參與費用" lazy-rules :rules="[rules.price]"/>
              <q-input class="col-12" filled v-model="form.place" label="活動地點" lazy-rules :rules="[rules.required]"/>
              <q-select class="col-12" filled :options="genres" v-model="form.genre" label="活動類型" :rules="[rules.required]" />
              <q-input class="col-12" filled v-model="form.description" label="揪團活動說明"
              clearable type="textarea" @keydown="processTextareaFill" @focus="processTextareaFill" :rules="[rules.required]"/>

              <q-file class="col-12" filled bottom-slots v-model="form.image" label="活動圖片" counter>
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

              <q-file class="col-12" filled v-model="form.images" label="請選擇主圖片(可複選)" use-chips multiple>
                <template v-slot:prepend>
                  <q-icon name="attach_file"></q-icon>
                </template>
              </q-file>

                <div class="row" >
                    <q-img class="q-ml-lg" v-for="image in images" :key="image" :src="image" width="100px" />
                </div>

              <q-card-section class="col-12">
                <q-input label="揪團日期、時間" filled v-model="form.date" :rules="[rules.required]">
                  <template v-slot:prepend>
                    <q-icon name="event" class="cursor-pointer">
                      <q-popup-proxy
                        cover
                        transition-show="scale"
                        transition-hide="scale"
                      >
                        <q-date v-model="form.date" mask="YYYY-MM-DD HH:mm">
                          <div class="row items-center justify-end">
                            <q-btn
                              v-close-popup
                              label="Close"
                              color="primary"
                              flat
                            ></q-btn>
                          </div>
                        </q-date>
                      </q-popup-proxy>
                    </q-icon>
                  </template>
                  <template v-slot:append>
                    <q-icon name="access_time" class="cursor-pointer">
                      <q-popup-proxy
                        cover
                        transition-show="scale"
                        transition-hide="scale"
                      >
                        <q-time
                          v-model="form.date"
                          mask="YYYY-MM-DD HH:mm"
                          format24h
                          now-btn
                        >
                          <div class="row items-center justify-end">
                            <q-btn v-close-popup label="Close" color="primary" flat />
                          </div>
                        </q-time>
                      </q-popup-proxy>
                    </q-icon>
                  </template>
                </q-input>
              </q-card-section>

              <q-select class="col-12 city" filled :options="categories" v-model="form.category" label="活動地點" :rules="[rules.required]" />
              <q-checkbox class="col-8" style="margin-bottom: 1.2rem;" v-model="form.sell" label="上架" />

              <q-btn class="col-6 Add-btn" flat label="Cancel" color="primary" v-close-popup />
              <q-btn class="col-6 Add-btn" flat label="Add Send" type="submit" :disabled="form.loading" color="primary" />
              </div>
            </q-form>

          </q-card>
        </q-dialog>
  </div>

</template>

<style scoped>
.add{
  position: absolute;
  right: 16%;
}

.q-card{
  width: 1200px;
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
.sumbitcard{
  width: 1200px;
}
.Add-btn{
  margin-bottom: 10px;
}
.city{
  margin-bottom: 20px;
}

</style>

<script setup>
import { apiAuth } from '../../boot/axios.js'
import { ref, reactive, computed } from 'vue'
import Swal from 'sweetalert2'

const categories = ['所有地區', '台北市', '新北市', '桃園市', '臺中市', '臺南市', '高雄市']
const genres = ['運動', '聊天', '吃飯', '散步', '唱歌', '活動', '露營', '旅行', '閱讀']

const gangActive = ref(categories[0])

const ac = computed(() => {
  return products.filter((fish1) => {
    if (gangActive.value === categories[0]) {
      return fish1
    } else {
      return fish1.category === gangActive.value
    }
  })
})

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
  date: '',
  math: 0,
  price: 0,
  place: '',
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
    form.date = ''
    form.math = 0
    form.price = 0
    form.place = ''
    form.description = ''
    form.image = undefined
    form.images = []
    form.genre = ''
    form.category = ''
    form.valid = false
    form.loading = false
    form.idx = -1
  } else {
    form._id = products[idx]._id
    form.name = products[idx].name
    form.date = products[idx].date
    form.math = products[idx].math
    form.price = products[idx].price
    form.place = products[idx].place
    form.description = products[idx].description
    form.image = products[idx].image
    form.images = products[idx].images
    form.genre = products[idx].genre
    form.category = products[idx].category
    form.valid = false
    form.loading = false
    form.idx = idx
  }
  form.dialog = true
}

const submit = async () => {
  form.loading = true
  const fd = new FormData()
  fd.append('name', form.name)
  fd.append('math', form.math)
  fd.append('price', form.price)
  fd.append('place', form.place)
  fd.append('date', form.date)
  fd.append('description', form.description)
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
    // const { data } = await apiAuth.get('/products/all')
    const { data } = await apiAuth.get('/products/user')
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
