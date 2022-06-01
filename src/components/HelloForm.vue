<template>
  <div>
    <form>
      <h3>Ввод пароля</h3>
      <p>
        <input v-model.trim="password" type="password" />
      </p>
      <h3>Повтор пароля</h3>
      <p>
        <input v-model.trim="repeatPassword" type="password" />
      </p>
      <p v-if="stateResult">Упс! Пароль не совпадает</p>
      <button @click="submitForm">Отправить</button>
    </form>
  </div>
</template>

<script>
/**
  /(?=.*[0-9])(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z!@#$%^&*]{6,}/g 
Пояснение:
(?=.*[0-9]) - строка содержит хотя бы одно число;
(?=.*[!@#$%^&*]) - строка содержит хотя бы один спецсимвол;
(?=.*[a-z]) - строка содержит хотя бы одну латинскую букву в нижнем регистре;
(?=.*[A-Z]) - строка содержит хотя бы одну латинскую букву в верхнем регистре;
[0-9a-zA-Z!@#$%^&*]{6,} - строка состоит не менее, чем из 6 вышеупомянутых символов.
*/

export default {
  name: "HelloForm",
  data() {
    return {
      password: "",
      repeatPassword: "",
      stateResult: false,
      passwordResult: {
        success: "",
        error: "",
      },
    };
  },
  methods: {
    submitForm(e) {
      // const regExp = new RegExp(
      //   "/(?=.*[0-9])(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z!@#$%^&*]{6,}/g"
      // );
      // console.log(regExp);
      const checkingLength =
        this.password.length === this.repeatPassword.length;

      e.preventDefault();
      if (checkingLength) {
        this.stateResult = false;
      } else {
        this.stateResult = true;
      }
    },
  },
};
</script>

<style scoped></style>
