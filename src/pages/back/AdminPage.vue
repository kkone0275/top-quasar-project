
  <template>
    <h5 class="text-center member">管理員頁面</h5>
    <section>
  <div class="column justify-center items-center q-mt-md">
    <q-avatar size="200px" color="secondary" text-color="white" class="q-mb-xl"  >
      <img src='https://source.boringavatars.com/beam/256/{{user.name}}colors=ffabab,ffdaab,ddffab,abe4ff,d9abff'>
    </q-avatar>

    <q-form @submit="onSubmit"  class="q-gutter-md text-center">
        <q-input class="in-set"
          outlined
          type="text"
          v-model="form.name"
          :label="$t('name')"
          lazy-rules
        />
        <q-input class="in-set"
          outlined
          type="text"
          v-model="form.account"
          :label="$t('account')"
          lazy-rules
        />
        <q-input class="in-set"
          outlined
          type="tel"
          v-model="form.email"
          :label="$t('email')"
          lazy-rules
        />
        <div>
          <!-- <q-btn label="儲存" type="submit" color="primary" /> -->
        </div>
      </q-form>
  </div>
</section>
</template>

<script setup>
// import { useUserStore } from '@/stores/user'
import { reactive } from 'vue'
import { useUserStore } from 'src/stores/user'
// import { useQuasar } from 'quasar'
// import { apiAuth } from 'src/boot/axios'
// const $q = useQuasar()
const user = useUserStore()
const form = reactive({
  name: user.name,
  account: user.account,
  email: user.email
})

</script>

<style>
q-input{
  width: 300px;
}
</style>
