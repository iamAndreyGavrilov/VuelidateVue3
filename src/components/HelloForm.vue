<template>
  <div>
    <form>
      <h3>Ввод пароля</h3>
      <p><input v-model.trim="password" type="password" /></p>
      <h3>Повтор пароля</h3>
      <p><input v-model.trim="repeatPassword" type="password" /></p>
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
      repeatPassword: "",
      passwordResult: {
        success: "",
        error: "",
      },
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
