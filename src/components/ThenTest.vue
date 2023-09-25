<script setup lang="ts">
import { ref } from "vue";

const mockFetch = ({ errorMode = false }: { errorMode?: boolean }) =>
  new Promise<string>((resolve, reject) => {
    setTimeout(() => {
      if (errorMode) {
        reject("失敗");

        return;
      }

      resolve("成功");
    }, 1000);
  });

const button1Status = ref("待機中");
const button1IsDisabled = ref(false);
const fetch1 = async () => {
  try {
    button1IsDisabled.value = true;
    button1Status.value = "処理中";
    button1Status.value = await mockFetch({});
  } catch (e) {
    button1Status.value = e as string;
  }
};

const button2Status = ref("待機中");
const button2IsDisabled = ref(false);
const fetch2 = async () => {
  try {
    button2IsDisabled.value = true;
    button2Status.value = "処理中";
    button2Status.value = await mockFetch({ errorMode: true });
  } catch (e) {
    button2Status.value = e as string;
  }
};

const button3Status = ref("待機中");
const button3IsDisabled = ref(false);
const fetch3 = async () => {
  try {
    button3IsDisabled.value = true;
    button3Status.value = "処理中";
    button3Status.value = await mockFetch({ errorMode: true });
  } catch (e) {
    button3Status.value = e as string;
    throw new Error("ここで再度エラーを投げる");
  }
};
</script>

<template>
  <div class="boxes">
    <div class="box box--good boxes__item">
      <p class="box__title">成功時に非活性になるボタン</p>
      <button
        @click="
          () =>
            fetch1()
              .then(() => (button1IsDisabled = true))
              .catch(() => (button1IsDisabled = false))
        "
        :disabled="button1IsDisabled"
        class="box__button"
      >
        実行
      </button>
      <p class="box__status">status: {{ button1Status }}</p>
    </div>
    <div class="box box--bad boxes__item">
      <p class="box__title">失敗時に非活性になるボタン</p>
      <button
        @click="
          () =>
            fetch2()
              .then(() => (button2IsDisabled = true))
              .catch(() => (button2IsDisabled = false))
        "
        :disabled="button2IsDisabled"
        class="box__button"
      >
        実行
      </button>
      <p class="box__status">status: {{ button2Status }}</p>
    </div>
    <div class="box box--good boxes__item">
      <p class="box__title">失敗時に活性になるボタン</p>
      <button
        @click="
          () =>
            fetch3()
              .then(() => (button3IsDisabled = true))
              .catch(() => (button3IsDisabled = false))
        "
        :disabled="button3IsDisabled"
        class="box__button"
      >
        実行
      </button>
      <p class="box__status">status: {{ button3Status }}</p>
    </div>
  </div>
</template>

<style scoped>
.boxes {
  display: grid;
  gap: 20px;
}
.box {
  display: grid;
  gap: 20px;
  background-color: #333;
  border-radius: 10px;
  padding: 30px;
}
.box--good {
  background-color: hsla(240, 50%, 25%, 1);
}
.box--bad {
  background-color: hsla(0, 50%, 25%, 1);
}
.box__title {
  margin: 0;
  font-size: 16px;
}
.box__status {
  margin: 0;
  opacity: 0.5;
  font-size: 12px;
}
.box__button {
  font-size: 16px;
}
</style>
