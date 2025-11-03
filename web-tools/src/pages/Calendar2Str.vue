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
          <div class="col-12"> <q-btn color="primary" icon="content_copy" @click="() => copyClipDate('M/D(ddd)')">{{
            date.formatDate(dateval, 'M/D(ddd)') }}</q-btn></div>
          <div class="col-12"> <q-btn color="primary" icon="content_copy"
              @click="() => copyClipDate('M.D<ddd>', enUS.date)">{{
                date.formatDate(dateval, 'M.D<ddd>', enUS.date) }}</q-btn></div>
          <div class="col-12"> <q-btn color="primary" icon="content_copy" @click="() => copyClipDate('M月D日dddd')">{{
            date.formatDate(dateval, 'M月D日dddd') }}</q-btn></div>
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
          <div class="col-12"> <q-btn color="primary" icon="content_copy"
              @click="() => copyToClipboard(date.formatDate(dateval + ' ' + timeval, 'M/D(ddd) HH:mm～'))">{{
                date.formatDate(dateval + ' ' + timeval, 'M/D(ddd) HH:mm～') }}</q-btn></div>
          <div class="col-12"> <q-btn color="primary" icon="content_copy"
              @click="() => copyToClipboard(date.formatDate(dateval + ' ' + timeval, 'M.D<ddd> HH:mm～', enUS.date))">{{
                date.formatDate(dateval + ' ' + timeval, 'M.D<ddd> HH:mm～', enUS.date) }}</q-btn></div>
          <div class="col-12"> <q-btn color="primary" icon="content_copy"
              @click="() => copyClipDateTime('M月D日dddd H時m分から')">{{
                date.formatDate(dateval + ' ' + timeval, 'M月D日dddd H時m分から') }}</q-btn></div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { copyToClipboard, type DateLocale, useQuasar, date } from 'quasar';
import { ref } from 'vue';
import enUS from 'quasar/lang/en-US.js'

const dateval = ref(date.formatDate(new Date(), 'YYYY/MM/DD'))
const timeval = ref(null)
const $q = useQuasar();

const copyClipDate = async (format: string, locale?: DateLocale) => {
  const str = date.formatDate(dateval.value, format, locale)
  await copyToClipboard(str)
  $q.notify({ message: "クリップボードへコピーしました: " + str, color: "positive" });
}
const copyClipDateTime = async (format: string, locale?: DateLocale) => {
  const str = date.formatDate(dateval.value + ' ' + timeval.value, format, locale);
  await copyToClipboard(str);
  $q.notify({ message: "クリップボードへコピーしました: " + str, color: "positive" })
}
</script>
