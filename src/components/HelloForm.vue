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

      <p v-show="statePassResultS">{{ passwordResult.success }}</p>

      <p v-show="statePassResultE">{{ passwordResult.error }}</p>

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
      statePassResultS: null,
      statePassResultE: null,
      passwordResult: {
        success: "",
        error: "",
      },
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

      const regExp = new RegExp("[0-9a-zA-Z!@#$%^&*]{6,}");

      if (!regExp.test(this.password)) {
        this.statePassResultS = null;
        this.passwordResult.error = "пароль не прошел валидацию";
        this.statePassResultE = true;
      } else {
        this.statePassResultE = null;
        this.passwordResult.success = "пароль прошел валидацию";
        this.statePassResultS = true;
      }

      this.v$.$validate();

      if (!this.v$.$error) {
        this.stateResult = false;
      } else {
        this.stateResult = true;
      }
    },
  },
};
</script>

<style scoped></style>
