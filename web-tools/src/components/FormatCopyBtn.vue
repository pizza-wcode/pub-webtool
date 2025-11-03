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
}>()
const $q = useQuasar();

const formattedStr = computed(() => {
  let str = ''
  if (props.withTime) {
    str = date.formatDate(props.dateval + ' ' + props.timeval, props.format, props.locale);
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
