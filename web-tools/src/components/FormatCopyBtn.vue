<template>
  <q-btn color="primary" icon="content_copy" @click="() => copyClipDate()" :no-caps="true">{{
    formattedStr }}</q-btn>
</template>

<script setup lang="ts">
import { copyToClipboard, type DateLocale, useQuasar, date } from 'quasar';
import { computed } from 'vue';
const props = defineProps<{
  dateval: string | undefined | null
  timeval: string | undefined | null
  withTime: boolean
  format: string,
  locale?: DateLocale
  upper?: boolean
  asahiruAM?: boolean
  hidePM?: boolean
  hour24?: boolean
  timestyle?: string
  gg120?: boolean
}>()
const $q = useQuasar();

const formattedStr = computed(() => {

  const format = (props.hour24 ? props.format : props.format.replace('H', 'h'))
  let str = ''
  if (props.withTime && props.timeval) {
    str = date.formatDate(props.dateval + ' ' + props.timeval, format, props.locale);
    const hour = Number.parseInt(props.timeval.slice(0, 2))
    const isAM = hour < 12;
    if (hour > 12 && props.hidePM && props.hour24) {
      str = str.replace('@1', '')
      str = str.replace('@2', '')
    } else if (props.timestyle == 'ahyy') {
      if (4 <= hour && hour <= 9) {
        str = str.replace('@1', '朝')
      } else if (10 <= hour && hour <= 15) {
        str = str.replace('@1', '昼')
      } else if (16 <= hour && hour <= 18) {
        str = str.replace('@1', '夕方')
      } else {
        str = str.replace('@1', '夜')
      }
      str = str.replace('@2', '')
    } else if (props.timestyle == 'gg') {
      if (isAM) {
        str = str.replace('@1', '午前')
      } else {
        str = str.replace('@1', '午後')
      }
      if (props.gg120) {
        str = str.replace('午前12', '午前0')
        str = str.replace('午後12', '午後0')
      }
      str = str.replace('@2', '')
    } else {
      str = str.replace('@1', '')
      if (isAM) {
        str = str.replace('@2', 'AM')
      } else {
        str = str.replace('@2', 'PM')
      }
    }

  } else {
    str = date.formatDate(props.dateval ?? '', format, props.locale);
  }
  if (props.upper) {
    return str.toUpperCase();
  } else {
    return str;
  }
})

const copyClipDate = async () => {
  await copyToClipboard(formattedStr.value)
  $q.notify({ message: "クリップボードへコピーしました: " + formattedStr.value, color: "positive" });
}
</script>
