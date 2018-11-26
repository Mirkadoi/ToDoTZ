<template>
<b-container class="main-vertical-center">
<div>
<b-form @submit="onSubmit" v-if="show">
      <b-form-group id="InputGroup1"
                    label="Email"
                    label-for="exampleInput1">
        <b-form-input id="Input1"
                      type="email"
                      v-model="form.email"
                      required
                      placeholder="Введите email">
        </b-form-input>
      </b-form-group>
            <b-form-group id="InputGroup2"
                    label="Пароль"
                    label-for="Input2">
        <b-form-input id="Input2"
                      type="password"
                      v-model="form.password"
                      required
                      placeholder="Введите пароль">
        </b-form-input>
      </b-form-group>
      <b-button type="submit" variant="primary">Войти</b-button>
    </b-form>
</div>
</b-container>
</template>

<script>
export default {
  data () {
    return {
      form: {
        email: '',
        password: '',
      },
      show: true
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault();
      alert(JSON.stringify(this.form));
    },
  }
}
</script>

<style scoped>
.main-vertical-center{
    display: flex;
    align-items: center;
    justify-content: center;
    height: 85vh;
}
</style>
