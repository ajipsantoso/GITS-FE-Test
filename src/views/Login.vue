<template>
  <div class="login">
    <div class="box">
      <div class="bg-circle">
      </div>
      <div class="box_header">
        <div class="circle logo-bg">
        </div>
        <div class="circle logo">
        </div>
        <div class="circle logo-inner">
        </div>
      </div>
      <div class="box_content">
        <v-form v-model="valid" ref="form" class="form-field">
          <v-layout justify-center column fill-height>
            <v-text-field
              label="Username"
              v-model="username"
              :rules="userRules"
              prepend-inner-icon="person"
              required
            ></v-text-field>
            <v-text-field
              v-model="password"
              :rules="passRules"
              prepend-inner-icon="lock_open"
              :type="show1 ? 'text' : 'password'"
              label="Password"
            ></v-text-field>
            <v-layout class="mb-3" align-content-space-between wrap>
              <v-checkbox
                v-model="checkbox"
                label="Remember me"
                class="input-remember"
                required
              ></v-checkbox>
              <!-- <v-spacer/> -->
              <router-link :to="{ path: '/forgot' }">Forget Password?</router-link>
            </v-layout>
            <v-layout column align-center justify-center>
            <v-btn
            class="btn-signin"
            v-bind:class="{ [`elevation-${6}`]: true }"
            @click="doLogin"
            >SIGN IN NOW<v-icon right color="pink">arrow_forward</v-icon></v-btn>
            <div>Don't have account? <b>Register</b></div>
            <!-- :class="{ red: !valid, green: valid }" -->
            </v-layout>
          </v-layout>
        </v-form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
// @ is an alias to /src

export default {
  name: 'login',
  data: () => ({
    valid: false,
    username: '',
    userRules: [
      v => !!v || 'Username is required',
    ],
    password: '',
    passRules: [
      v => !!v || 'Password is required',
    ],
    show1: false,
    checkbox: '',
  }),
  methods: {
    doLogin() {
      if (this.$refs.form.validate()) {
        const BASE_URL = 'http://207.148.79.65:9021/v1/user/login';
        axios
          .post(BASE_URL, {
            email: this.username,
            password: this.password,
          },
          {
            headers: {
              'Content-Type': 'application/json',
            },
          })
          .then((snap) => {
            // eslint-disable-next-line
            alert(snap.data.message);
            if (snap.data.message === 'success') {
              this.$router.push({ path: '/registrasi' });
            }
            console.log(snap);
          })
          .catch((err) => {
            console.log(err);
          });
      }
    },
  },
};
</script>

<style>
  .login{
    height: 100%;
    background: linear-gradient(45deg, #a623cc 0%, #ff6c9a 80%);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .box{
    min-width: 60%;
    min-height: 70%;
    border-radius: 20px;
    background: white;
    position: relative;
  }
  .box .bg-circle{
    width: 52%;
    height: 100%;
    border-radius: 20px 40% 40% 20px / 20px 50% 50% 20px;
    box-shadow: 0 0 5px rgba(17, 17, 17, 0.342);
    background: #f6f7fc;
    position: absolute;
  }
  .box_header{
    position:relative;
    margin: 50px auto;
    z-index: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .box_content{
    margin-bottom: 50px;
    position:relative;
    z-index: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .circle{
    border-radius: 50%;
    background: #fff;
  }
  .circle.logo-bg{
    width: 90px;
    height: 90px;
    box-shadow: 0 0 5px rgba(17, 17, 17, 0.342);
  }
  .circle.logo{
    position: absolute;
    width: 65px;
    height: 65px;
    background: linear-gradient(#ff6c9a, #a623cc);
  }
  .circle.logo-inner{
    position: absolute;
    width: 45px;
    height: 45px;
  }
  .form-field{
    width: 40%;
  }
  a{
    color: black;
  }
  .v-input{
    margin-top:20px;
    padding-top:0;
  }
  .v-input.input-remember{
    margin-top: unset;
  }
  .v-btn.btn-signin{
    height: 50px;
    width: 300px;
    border-radius: 50px;
    margin-bottom: 20px;
  }
</style>
