<template>
  <div>
    <form>
      <h3>Пароль</h3>
      <p><input v-model.trim="password" type="password" /></p>
      <h3>Подтверждение пароля</h3>
      <p><input v-model.trim="passwordСheck" type="password" /></p>
      <button @click="submitForm" type="submit">Отправить</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "HelloForm",
  data() {
    return {
      password: "",
      passwordСheck: "",
    };
  },
  methods: {
    submitForm(e) {
      e.preventDefault();
    },
  },
};
</script>

<style scoped></style>
