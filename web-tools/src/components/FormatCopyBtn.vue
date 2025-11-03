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
  timestyle?: string
}>()
const $q = useQuasar();

const formattedStr = computed(() => {
  let str = ''
  if (props.withTime && props.timeval) {
    str = date.formatDate(props.dateval + ' ' + props.timeval, props.format, props.locale);
    const hour = Number.parseInt(props.timeval.slice(0, 2))
    const isAM = hour < 12;
    if (!isAM && props.hidePM) {
      str = str.replace('@', '')
    } else if (props.timestyle == 'ahyy') {
      if (4 <= hour && hour <= 9) {
        str = str.replace('@', '朝')
      } else if (10 <= hour && hour <= 16) {
        str = str.replace('@', '昼')
      } else if (16 <= hour && hour <= 17) {
        str = str.replace('@', '夕方')
      } else {
        str = str.replace('@', '夜')
      }
    } else if (props.timestyle == 'gg') {
      if (isAM) {
        str = str.replace('@', '午前')
      } else {
        str = str.replace('@', '午後')
      }
    } else {
      if (isAM) {
        str = str.replace('@', 'AM')
      } else {
        str = str.replace('@', 'PM')
      }

    }

  } else {
    str = date.formatDate(props.dateval ?? '', props.format, props.locale);
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
