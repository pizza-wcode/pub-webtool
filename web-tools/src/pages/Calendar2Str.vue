<template>
  <q-page class="q-ma-lg">
    <div class="row items-start justify-start q-col-gutter-md">
      <div class="col-12 col-md-auto">
        <div class="text-bold"> 日付選択</div>
        <q-date v-model="dateval" :landscape="true"></q-date>
        <br />
        <span style="color: grey;">
          カレンダーから日付をクリック<br />
          月や年を変えたい時は白いパネルの上部にある<br />
          ＜＞で囲まれた数字をクリック</span>

      </div>
      <div class="col-12 col-md">
        <div class="row q-col-gutter-md">
          <div class="col-12 text-bold">日付文字をクリップボードにコピー</div>

          <div class="col-12"> <format-copy-btn format="M/D(ddd)" :with-time="false" :dateval="dateval"
              :timeval="timeval" />
          </div>
          <div class="col-12">
            <format-copy-btn format="M.D<ddd>" :with-time="false" :dateval="dateval" :timeval="timeval"
              :locale="enUS.date" :upper="true" />
            <format-copy-btn format="M.D<ddd>" :with-time="false" :dateval="dateval" :timeval="timeval"
              :locale="enUS.date" class="q-mx-md" />
          </div>
          <div class="col-12">
            <format-copy-btn format="M月D日dddd" :with-time="false" :dateval="dateval" :timeval="timeval" />
          </div>
        </div>
      </div>
    </div>
    <div class="row items-start justify-start q-col-gutter-md q-mt-lg">


      <div class="col-12 col-md-auto">
        <div class="text-bold"> 時刻選択</div>
        <q-time v-model="timeval" :landscape="true" :format24h="true"
          :minute-options="[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55]"></q-time>
        <br />
        <span style="color: grey;">
          時、分の順番に数字をクリックして選択<br />
          分を選択するタイミングで時を変更したい時は<br />左側の青いパネルで色が薄くなっている時の数字をクリック</span>

      </div>
      <div class="col-12 col-md">
        <div class="row q-col-gutter-md" v-if="timeval">
          <div class="col-12 text-bold">日付文字をクリップボードにコピー</div>
          <div class="col-4">
            <q-select v-model="timestyle" label="AMPMスタイル"
              :options="[{ label: '朝昼夕方夜', value: 'ahyy' }, { label: '午前/午後', value: 'gg' }, { label: 'AM/PM', value: 'ampm' }]"
              :emit-value="true" :map-options="true" />
          </div>
          <div class="col-12">
            <q-checkbox v-model="hidePM" label="12時以降は「午後」を省略" />
          </div>
          <div class="col-12">
            <format-copy-btn format="M/D(ddd) @H:mm～" :with-time="true" :dateval="dateval" :timeval="timeval"
              :hide-p-m="hidePM" :timestyle="timestyle" />
          </div>
          <div class="col-12">
            <format-copy-btn format="M.D<ddd> @H:mm～" :with-time="true" :dateval="dateval" :timeval="timeval"
              :upper="true" :locale="enUS.date" :hide-p-m="hidePM" :timestyle="timestyle" />
            <format-copy-btn format="M.D<ddd> @H:mm～" :with-time="true" :dateval="dateval" :timeval="timeval"
              :locale="enUS.date" class="q-mx-md" :hide-p-m="hidePM" :timestyle="timestyle" />
          </div>
          <div class="col-12">
            <format-copy-btn format="M月D日dddd @H時m分から" :with-time="true" :dateval="dateval" :timeval="timeval"
              :hide-p-m="hidePM" :timestyle="timestyle" />
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { date } from 'quasar';
import { ref } from 'vue';
import enUS from 'quasar/lang/en-US.js'
import FormatCopyBtn from 'src/components/FormatCopyBtn.vue';

const dateval = ref(date.formatDate(new Date(), 'YYYY/MM/DD'))
const timeval = ref(null)
const hidePM = ref(true)
const timestyle = ref<string>('ahyy')
</script>
