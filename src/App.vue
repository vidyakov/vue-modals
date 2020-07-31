<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <div id="app">

            <!--First modal-->
            <button class="btn btnPrimary" @click="simpleModalShow = true">Open simple modal</button>
            <modal
                    v-show="simpleModalShow"
                    title="Simple Modal"
                    @close="simpleModalShow = false"
            >
              <div slot="body">
                <p slot="body">Slot text. Slot text. Slot text. Slot text.</p>
                <button class="btn btnDefaul" @click="simpleModalShow = false">Ok</button>
              </div>
            </modal>

            <!--Second modal-->
            <button class="btn btnPrimary" @click="modalForm.show = true">Open modal form</button>
            <modal
                    v-show="modalForm.show"
                    title="Modal form"
                    @close="modalForm.show = false"
            >
              <form slot="body" @submit.prevent="submitForm(modalForm, ['name', 'email'])">
                <label>
                  Name:
                  <input type="text" v-model="modalForm.name" placeholder="Sasha">
                </label>
                <label>
                  Email:
                  <input type="text" v-model="modalForm.email" placeholder="sasha@gmail.com">
                </label>
                <button class="btn btnWhite">Send data to console</button>
              </form>
            </modal>

            <!--Third modal-->
            <button class="btn btnPrimary" @click="showFormWithValidation = true">Open modal form with validation</button>
            <modal
                    v-show="showFormWithValidation"
                    title="Modal form with validate"
                    @close="showFormWithValidation = false"
            >
              <form slot="body" @submit.prevent="submitFormWithValidation">
                <label>
                  Name:
                  <input
                          type="text"
                          v-model="name"
                          placeholder="Sasha"
                          @change="$v.name.$touch()"
                          :class="{error: $v.name.$error}"
                  >
                </label>

                <label>
                  Email:
                  <input
                          type="text"
                          v-model="email"
                          placeholder="sasha@gmail.com"
                          @change="$v.email.$touch()"
                          :class="{error: $v.email.$error}"
                  >
                </label>

                <label>Password:
                  <input
                          type="password"
                          v-model="password"
                          @change="$v.password.$touch()"
                          :class="{error: $v.password.$error}"
                          placeholder="qwertyqwerty"
                  >
                </label>

                <label>Repeat password:
                  <input
                          type="password"
                          v-model="repeatPassword"
                          @change="$v.repeatPassword.$touch()"
                          :class="{error: $v.repeatPassword.$error}"
                          placeholder="qwertyqwerty"
                  >
                </label>
                <button class="btn btnWhite">Send data to console</button>
              </form>
            </modal>

          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
import modal from './components/Modal.vue'

export default {
  name: 'App',
  components: {
    modal
  },
  data () {
    return {
      simpleModalShow: false,
      modalForm: {
        show: false,
        name: '',
        email: ''
      },
      showFormWithValidation: false,
      name: '',
      email: '',
      password: '',
      repeatPassword: ''
    }
  },
  methods: {
    submitForm (form, items) {
      for (let item of items) {
        console.log(`${item}: ${form[item]}`);
        form[item] = ''
      }
      form.show = false;
    },
    submitFormWithValidation () {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        console.log('Name: ', this.name);
        console.log('Email: ', this.email);

        this.showFormWithValidation = false;
        this.name = '';
        this.email = '';
        this.$v.$reset();
      }
    }
  },
  validations: {
    name: {
      minLength: minLength(4),
      required
    },
    email: {
      email,
      required
    },
    password: {
      minLength: minLength(6),
      required
    },
    repeatPassword: {
      sameAs: sameAs('password')
    }
  }
}
</script>

<style>
  .error {
    border-color: #d05243;
  }
</style>
