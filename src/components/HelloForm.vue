<template>
  <div class="main">
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
      <li class="main__li" v-for="error in errors" :key="error">{{ error }}</li>
      <p>
        <input
          v-model.trim="repeatPassword"
          type="password"
          placeholder="Повтор пароля"
        />
      </p>

      <p :class="{ red: stateResult }" v-show="stateResult">
        Пароль не совпадает
      </p>

      <p :class="{ red: !passwordBoolean, green: passwordBoolean }">
        {{ passwordResult }}
      </p>

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
      passwordBoolean: null,
      errors: [],
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

      const regExpNum = new RegExp("(?=.*[0-9])");
      const regExpSymbol = new RegExp("(?=.*[!@#$%^&*])");
      const regExpLower = new RegExp("(?=.*[a-z])");
      const regExpUpper = new RegExp("(?=.*[A-Z])");

      if (!regExpNum.test(this.password)) {
        this.errors.push("введите хотя бы одно число");
      }
      if (!regExpSymbol.test(this.password)) {
        this.errors.push("введите хотя бы один спецсимвол");
      }
      if (!regExpLower.test(this.password)) {
        this.errors.push(
          "введите хотя бы одну латинскую букву в нижнем регистре"
        );
      }
      if (!regExpUpper.test(this.password)) {
        this.errors.push("бы одну латинскую букву в верхнем регистре");
      }

      // указывать на конкретную ошибку
      const regExp = new RegExp(
        "((?=.*\\d)(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z]).{6,20})"
      );

      this.passwordResult = regExp.test(this.password)
        ? "пароль прошел валидацию"
        : "пароль не прошел валидацию";

      this.passwordBoolean = regExp.test(this.password);

      this.v$.$validate();
      this.stateResult = this.v$.$error;
    },
  },
};
</script>

<style scoped>
* {
  font-size: 10px;
}
.main {
  width: 200px;
  height: 100px;
  margin: auto;
  margin-top: 50px;
}
.red {
  color: red;
}
.green {
  color: green;
}
.main__li {
  font-size: 6px;
  color: red;
}
</style>
