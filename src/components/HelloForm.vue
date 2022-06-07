<template>
  <div>
    <form>
      <p>
        Пароль должен содержать: число, спецсимвол, латинскую букву в нижнем
        регистре, латинскую букву в верхнем регистре, быть более 6 символов
      </p>
      <p>
        <input
          v-model.trim="password"
          type="password"
          placeholder="Ввод пароля"
        />
      </p>
      <p>
        <input
          v-model.trim="repeatPassword"
          type="password"
          placeholder="Повтор пароля"
        />
      </p>

      <p v-show="stateResult">Упс! Пароль не совпадает</p>

      <p>{{ passwordResult }}</p>

      <button @click="submitForm">Отправить</button>
    </form>
  </div>
</template>

<script>
// успешный пароль - зеленый, не успешный красный

/**
  ((?=.*\\d)(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z]).{6,20})
Пояснение:
(?=.*[0-9]) - строка содержит хотя бы одно число;
(?=.*[!@#$%^&*]) - строка содержит хотя бы один спецсимвол;
(?=.*[a-z]) - строка содержит хотя бы одну латинскую букву в нижнем регистре;
(?=.*[A-Z]) - строка содержит хотя бы одну латинскую букву в верхнем регистре;
*/

import useVuelidate from "@vuelidate/core";
import { required, sameAs } from "@vuelidate/validators";

export default {
  name: "HelloForm",
  data() {
    return {
      v$: useVuelidate(),
      password: "",
      repeatPassword: "",
      stateResult: null,
      passwordResult: "",
    };
  },
  validations() {
    return {
      password: { required },
      repeatPassword: { required, sameAs: sameAs(this.password) },
    };
  },
  watch: {
    //можно в реалтайме отслеживать
  },
  methods: {
    submitForm(e) {
      e.preventDefault();
      // указывать на конкретную ошибку
      const regExp = new RegExp(
        "((?=.*\\d)(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z]).{6,20})"
      );

      // if (!regExp.test(this.password)) {
      //   this.passwordResult = "пароль не прошел валидацию";
      // } else {
      //   "пароль прошел валидацию";
      // }

      this.passwordResult = regExp.test(this.password)
        ? "пароль прошел валидацию"
        : "пароль не прошел валидацию";

      this.v$.$validate();
      this.stateResult = this.v$.$error;
    },
  },
};
</script>

<style scoped></style>
