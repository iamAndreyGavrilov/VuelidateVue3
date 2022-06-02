<template>
  <div>
    <form>
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
      <p v-if="stateResult">Упс! error</p>
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
import { required, minLength, sameAs } from "@vuelidate/validators";

export default {
  name: "HelloForm",
  data() {
    return {
      v$: useVuelidate(),
      password: "",
      repeatPassword: "",
      stateResult: false,
      passwordResult: {
        success: "",
        error: "",
      },
    };
  },
  validations() {
    return {
      password: { required, minLength: minLength(6) },
      repeatPassword: { required, sameAs: sameAs(this.password) },
    };
  },
  methods: {
    submitForm(e) {
      e.preventDefault();

      this.v$.$validate();

      if (!this.v$.$error) {
        this.stateResult = false;
      } else {
        this.stateResult = true;
      }

      // const checkingLength =
      //   this.password.length === this.repeatPassword.length;

      // if (checkingLength) {
      //   this.stateResult = false;
      // } else {
      //   this.stateResult = true;
      // }
    },
  },
};
</script>

<style scoped></style>
