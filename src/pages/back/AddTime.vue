<template>
  <h5 class="text-center">填空上架</h5>
  <q-btn class="add" style="background: #F3A308; color: white" @click="openAdd(-1)" label="新增揪團" />

  <table class="box" style="width: 60%; " border="1">
    <thead>
      <tr align="left">
        <th>空閒時間</th>
        <th>個人暱稱</th>
        <th>活動地點</th>
        <th>管理</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(time, idx) in times" :key="time._id">
        <!-- <td align="center">
          <img :src="time.image" :aspect-ratio="1" :width="100" :height="100"
          style="object-fit: cover; margin: auto;">
        </td> -->
        <td>{{ time.date }}</td>
        <td>{{ time.name }}</td>
        <td>{{ time.category }}</td>
        <td align="center">
          <q-btn color="primary" icon="edit" @click="openAdd(idx)" />
        </td>
      </tr>
    </tbody>
  </table>

  <div class="q-pa-md">
    <div class="row justify-center">
      <div class="col-12 col-md-1">
        <q-dialog v-model="form.dialog" persistent>
          <q-card style="max-width: 800px ">
            <div class="text-h6" align="center">{{ form._id.length > 0 ? '編輯填空' : '新增填空' }}</div>

            <q-form @submit="submit">
              <div class="flex row justify-between">
              <q-input class="col-12" style="padding:10px ;" filled v-model="form.name" label="個人暱稱" lazy-rules :rules="[rules.required]"/>
              <!-- <q-input class="col-12" style="padding:10px ;" filled v-model="form.price" label="活動價格" lazy-rules :rules="[rules.price]"/> -->
              <q-input class="col-12" style="padding: 10px;" filled v-model="form.description" label="空閒描述"
              clearable type="textarea" @keydown="processTextareaFill"
              @focus="processTextareaFill"
              :rules="[rules.required]"/>

      <q-card-section class="col-12">
        <q-input label="活動日期、時間" filled v-model="form.date" :rules="[rules.required]">
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

      <q-input class="col-3" style="padding:10px ;" filled v-model="form.hour" label="活動小時數" lazy-rules :rules="[rules.required]"/>

              <q-select class="col-9 city" filled :options="categories" v-model="form.category" label="活動地點" :rules="[rules.required]" />

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

.city{
  position: relative;
  bottom: 15px;
}
</style>

<script setup>
import { apiAuth } from '../../boot/axios.js'
import { reactive } from 'vue'
import Swal from 'sweetalert2'

const categories = ['台北市', '新北市', '桃園市', '臺中市', '臺南市', '高雄市']
const rules = {
  required (value) {
    return !!value || '欄位必填'
  }
}

const times = reactive([])
const form = reactive({
  _id: '',
  name: '',
  date: '',
  hour: 0,
  description: '',
  sell: false,
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
    form.hour = 0
    form.description = ''
    form.category = ''
    form.valid = false
    form.loading = false
    form.idx = -1
  } else {
    form._id = times[idx]._id
    form.name = times[idx].name
    form.date = times[idx].date
    form.hour = times[idx].hour
    form.description = times[idx].description
    form.category = times[idx].category
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
  fd.append('date', form.date)
  fd.append('hour', form.hour)
  fd.append('description', form.description)
  fd.append('category', form.category)

  try {
    if (form._id.length === 0) {
      const { data } = await apiAuth.post('/times', fd)
      times.push(data.result)
      Swal.fire({
        icon: 'success',
        title: '成功',
        text: '新增成功'
      })
    } else {
      const { data } = await apiAuth.patch('/times/' + form._id, fd)
      times[form.idx] = data.result
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
    // const { data } = await apiAuth.get('/times/all')
    const { data } = await apiAuth.get('/times/user')
    times.push(...data.result)
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: '失敗',
      text: error?.response?.data?.message || '發生錯誤'
    })
  }
})()
</script>
