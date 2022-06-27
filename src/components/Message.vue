<template>
  <div class="message">
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      require: true,
      //   default: "default message",
    },
  },
};
</script>

<style lang="scss" scoped>
.message {
  padding: 20px 0;
}
p {
  color: #ba3838;
  text-align: center;
}
</style>
// scope - ограничивает область видимости стилей, стили будут применены только к
текущему компоненту
