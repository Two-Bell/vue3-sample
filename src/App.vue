<script lang="ts">
import {
  ref,
  computed,
  ComponentPublicInstance,
  getCurrentInstance,
  onMounted,
} from "vue";
import TopImage from "./components/topImage.vue";
import TextCount from "./components/TextCount.vue";
import Calendar from "./components/Calendar.vue";
import type { CalendarComponent } from "./components/Calendar.vue";
import { testJson } from "./helper/testJson";
</script>
<script setup lang="ts">
// 하위 component의 함수나 변수 상수들을 가져다 쓸 수 있게 되었음
// 다만 component의 type을 ComponentPublicInstance 해당 값이 정확히 맞는지는 조금 더 확인이 필요
const data = ref<CalendarGroup>(testJson);
const calendarCp =
  ref<ComponentPublicInstance<typeof Calendar & CalendarComponent>>();

onMounted(() => {
  // 아래와 같이 사용할 바엔 hook 또는 더 좋은 방안이 있을 듯
  console.log(getCurrentInstance()?.appContext.config.globalProperties.$test);
});

const handle = (day: number) => {
  const { list } = data.value;
  if (list[day - 1].yn) return alert("이미 출석체크 완료");

  data.value.total++;
  data.value.list[day - 1].yn = true;
  calendarCp.value?.onClickTest();
};
</script>

<template>
  <div id="container">
    <TopImage />
    <TextCount :total="data.total" />
    <Calendar ref="child" @onClick="handle" :list="data.list" />
  </div>
</template>

<style scoped></style>
