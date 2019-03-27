<template>
  <div class="register">
    <div class="navbar">
      <router-link class="power-link" :to="{ path: '/' }">
        <v-icon class="navbar_power" x-large color="white">power_settings_new</v-icon>
      </router-link>
    </div>
    <v-stepper v-model="step">
    <v-stepper-header class="v-steper-header">
      <v-stepper-step class="steper-step" :complete="step >= 1"
        :edit-icon="'power_settings_new'" step="1"></v-stepper-step>
      <v-divider :class="{step_pass: step > 1, step_notpass: step <= 1}"></v-divider>
      <v-stepper-step class="steper-step" :complete="step > 2" step="2"></v-stepper-step>
      <v-divider :class="{step_pass: step > 2, step_notpass: step <= 2}"></v-divider>
      <v-stepper-step class="steper-step" :complete="step > 3" step="3"></v-stepper-step>
      <v-divider :class="{step_pass: step > 3, step_notpass: step <= 3}"></v-divider>
      <v-stepper-step class="steper-step" :complete="step > 4" step="4"></v-stepper-step>
      <v-divider :class="{step_pass: step > 4, step_notpass: step <= 4}"></v-divider>
      <v-stepper-step class="steper-step" step="5"></v-stepper-step>
    </v-stepper-header>
    <v-stepper-items>
      <v-stepper-content class="pa-2" step="2">
        <v-layout class="mb-3">
          <div class="title-desc">Province</div>
        </v-layout>
        <v-card
          class="mb-5 v-card-province"
          flat
          light
        >
          <v-window v-model="onboarding">
            <v-window-item
              v-for="n in 1"
              :key="`card-${n}`"
            >
              <v-card
                flat
                color="transparent"
              >
                <v-layout
                  align-center
                  justify-center
                  fill-height
                  wrap
                  tag="v-card-text"
                >
                  <div v-for="(item, idx) in items_province" :key="idx" class="form-check">
                    <input class="form-check-input" v-model="user_data.province"
                    type="radio" :name="`gridTime`" :id="`gridTime${idx}`"
                    :value="item">
                    <label v-ripple class="form-check-label time" :for="`gridTime${idx}`">
                            <div class="label-desc">
                                <h5>{{item}}</h5>
                            </div>
                    </label>
                  </div>
                </v-layout>
              </v-card>
            </v-window-item>
          </v-window>

          <v-card-actions class="justify-space-between">
            <v-btn
              fab small
              color="grey lighten-2"
              @click="prev"
            >
              <v-icon>fas fa-chevron-left</v-icon>
            </v-btn>
            <v-btn
              fab small
              color="grey lighten-2"
              @click="next"
            >
              <v-icon>fas fa-chevron-right</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
        <v-layout justify-space-around>
            <v-btn
              class="btn-round"
              outline
              depressed
              color="#EC407A"
              @click="step = 2"
            ><v-icon left>arrow_back</v-icon>
              BACK
            </v-btn>
          <v-btn
            class="btn-round"
            @click="step = 3"
            depressed
            dark
            color="#EC407A">NEXT<v-icon right>arrow_forward</v-icon></v-btn>
        </v-layout>
      </v-stepper-content>
      <v-stepper-content class="pa-2" step="3">
        <v-layout class="mb-3">
          <div class="title-desc">Address</div>
        </v-layout>
        <v-card
          class="mb-5 px-5 v-card-address"
          flat
        >
          <div class="v-data">
            <div class="data-city">
              <div class="mb-1">City</div>
              <v-select
                solo
                flat
                background-color="grey lighten-2"
                class="v-input-custom"
                :items="items_city"
                v-model="user_data.city"
              ></v-select>
            </div>
            <div class="data-zip">
              <div class="mb-1">Zip Code</div>
              <v-text-field
                solo
                flat
                class="v-input-custom"
                background-color="grey lighten-2"
                v-model="user_data.zip"
              ></v-text-field>
            </div>
            <div class="data-address address">
              <div class="mb-1">Complete Address</div>
              <v-textarea
                solo
                auto-grow
                flat
                class="v-input-custom"
                background-color="grey lighten-2"
                name="input"
                v-model="user_data.address"
              ></v-textarea>
            </div>
          </div>
        </v-card>
        <v-layout justify-space-around>
            <v-btn
              class="btn-round"
              outline
              depressed
              color="#EC407A"
              @click="step = 2"
            ><v-icon left>arrow_back</v-icon>
              BACK
            </v-btn>
          <v-btn
          class="btn-round"
          @click="step = 4"
          depressed
          dark
          color="#EC407A"
          >NEXT<v-icon right>arrow_forward</v-icon></v-btn>
        </v-layout>
      </v-stepper-content>

      <v-stepper-content class="pa-2" step="4">
        <v-layout class="mb-3">
          <div class="title-desc">Upload Profile Picture</div>
        </v-layout>
        <v-card
          class="mb-5"
          flat
        >
          <v-layout column align-center>
            <v-avatar
              class="mb-3"
              size="225px"
              color="grey lighten-4"
            >
              <img :src="imageUrl" height="150" v-if="imageUrl"/>
              <i class="icon-image far fa-user" v-else></i>
            </v-avatar>
            <v-btn
            class="btn-round"
            large
            @click='pickFile'
            depressed
            dark
            color="#EC407A">UPLOAD</v-btn>
            <input
              type="file"
              style="display: none"
              ref="image"
              accept="image/*"
              @change="onFilePicked"
            >
          </v-layout>
        </v-card>
        <v-layout justify-space-around>
            <v-btn
              class="btn-round"
              outline
              depressed
              color="#EC407A"
              @click="step = 3"
            ><v-icon left>arrow_back</v-icon>
              BACK
            </v-btn>
          <v-btn
            class="btn-round"
            @click="checkData();step = 5;"
            depressed
            dark
            color="#EC407A">NEXT<v-icon right>arrow_forward</v-icon></v-btn>
        </v-layout>
      </v-stepper-content>

      <v-stepper-content step="5">
        <v-card
          class="mb-5"
          flat
        >
        <v-layout fill-height justify-center align-center column>
          <img style="height:250px" src="../assets/key.png" alt="Success">
          <div class="success_msg success_title">Congratulation!</div>
          <div class="success_msg">We have send you email to verify your account!</div>
        </v-layout>
        </v-card>
        <v-layout justify-space-around>
            <v-btn
              class="btn-round"
              outline
              depressed
              color="#EC407A"
              @click="step = 4"
            ><v-icon left>arrow_back</v-icon>
              BACK
            </v-btn>
          <v-btn
            class="btn-round"
            depressed
            dark
            color="#EC407A"
            @click="step = 2"
            >NEXT<v-icon right>arrow_forward</v-icon></v-btn>
        </v-layout>
      </v-stepper-content>
    </v-stepper-items>
  </v-stepper>
  </div>
</template>

<script>
import axios from 'axios';
// @ is an alias to /src
const BASE_URL = 'http://dev.gits.id:9021/v1/profile';

export default {
  name: 'register',
  data: () => ({
    imageName: '',
    imageUrl: '',
    imageFile: '',
    step: 2,
    user_data: {
      province: '',
      imageFile: '',
      address: '',
      zip: '',
      city: '',
    },
    items_province: [
      'Jawa Barat',
      'Jawa Tengah',
      'Jawa Timur',
      'Bali',
      'Yogyakarta',
      'NTB',
      'NTT',
      'Papua',
    ],
    items_city: [
      'Jakarta Pusat',
      'Denpasar',
      'Surabaya',
    ],
    length: 3,
    onboarding: 0,
  }),
  methods: {
    checkData() {
      console.log(this.user_data);
    },
    setIcon() {
      const stepper = document.querySelectorAll('span.v-stepper__step__step');
      stepper[1].innerHTML = '<i class="fas fa-map-marker-alt"></i>';
      for (let i = 2; i < stepper.length; i += 1) {
        stepper[i].innerHTML = '';
      }
    },
    pickFile() {
      this.$refs.image.click();
    },
    onFilePicked(e) {
      const files = e.target.files;
      if (files[0] !== undefined) {
        this.imageName = files[0].name;
        if (this.imageName.lastIndexOf('.') <= 0) {
          this.imageName = '';
          this.imageFile = '';
          this.imageUrl = '';
        } else {
          const fr = new FileReader();
          fr.readAsDataURL(files[0]);
          fr.addEventListener('load', () => {
            this.imageUrl = fr.result;
            this.imageFile = files[0]; // this is an image file that can be sent to server...
            this.user_data.imageFile = files[0];
          });
        }
      } else {
        this.imageName = '';
        this.imageFile = '';
        this.imageUrl = '';
      }
    },
    next() {
      this.onboarding = this.onboarding + 1 === this.length
        ? 0
        : this.onboarding + 1;
    },
    prev() {
      this.onboarding = this.onboarding - 1 < 0
        ? this.length - 1
        : this.onboarding - 1;
    },
    getProvince() {
      axios
        .get(`${BASE_URL}/provinces/1`)
        .then((snap) => {
          console.log(snap);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  mounted() {
    this.setIcon();
    this.getProvince();
  },
  watch: {
    step(newValue) {
      const stepper = document.querySelectorAll('span.v-stepper__step__step');
      if (newValue === 2) {
        for (let i = 2; i < stepper.length; i += 1) {
          stepper[i].innerHTML = '';
        }
      } else {
        const arr = [
          '<i class="fas fa-map-marker-alt"></i>',
          '<i class="fas fa-map-signs"></i>',
          '<i class="fas fa-user"></i>',
          '<i aria-hidden="true" class="v-icon material-icons theme--light">check</i>'];
        for (let i = 0; i < this.step; i += 1) {
          if (i === newValue - 1) {
            stepper[i].innerHTML = arr[i - 1];
          } else {
            stepper[i].innerHTML = '<i aria-hidden="true" class="v-icon material-icons theme--light">check</i>';
          }
        }
      }
    },
  },
};
</script>

<style>
  .register{
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .navbar{
    width: 100%;
    height: 80px;
    top: 0;
    background: linear-gradient(-90deg, #a623cc 0%, #ff6c9a 80%);
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }
  .navbar a.power-link{
    text-decoration: none;
    margin-right: 50px;
  }
  .v-stepper{
    width: 80%;
  }
  .steper-step{
    padding: unset;
  }
  .v-stepper__step__step{
    margin-right: 0;
    width: 40px;
    height: 40px;
  }
  .v-steper-header{
    box-shadow: unset;
    padding: 25px 30px;
    height: auto;
  }
  .theme--light.v-stepper .v-stepper__step:not(
    .v-stepper__step--active):not(
    .v-stepper__step--complete):not(
    .v-stepper__step--error) .v-stepper__step__step{
    background: rgb(216, 216, 216);
  }
  .step_pass{
    background: linear-gradient(90deg, #a623cc 0%, #ff6c9a 80%);
    max-height: unset;
    height: 10px;
    border: unset;
  }
  .step_notpass{
    background: rgb(216, 216, 216);
    max-height: unset;
    height: 10px;
    border: unset;
  }
  .title-desc{
    margin-left: 12%;
    height: 200%;
    font-size: 24px;
    text-align: center;
    color: #e72c83;
    font-weight: bold;
  }
  .primary{
    background: linear-gradient(-45deg, #a623cc 0%, #ff6c9a 80%);
  }
  .v-btn.btn-round{
    border-radius: 25px;
  }
  .v-card{
    min-height: 350px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .v-card-province{
    flex-wrap: wrap;
    align-items: flex-start;
  }
  .v-card-address .v-data{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    width: 70%;
  }
  .form-check{
    margin: 15px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .form-check-label{
    user-select: none;
    padding: 20px 10px;
    min-width:200px;
    border-radius: 20px;
    border: #a623cc solid 2px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    color: #a623cc;
    font-size: 24px;
    /* transition: linear 0.25s; */
  }
  input[type="radio"]{
    visibility:hidden;
  }
  input:checked + .form-check-label{
    background: linear-gradient(0deg, #a623cc, #e182fc);
    border: unset;
    color: #fff;
  }
  .v-input-custom.v-text-field--solo > .v-input__control > .v-input__slot{
    border-radius: 30px;
  }
  .data-address.address{
   width: 90%;
  }
  .icon-image{
    font-size: 88px;
    background: linear-gradient(to bottom, #e72c83 0%,#a742c6 100%);
    background-clip: text;
    -webkit-text-fill-color:transparent;
  }
  .success_title{
    color: #e72c83;
    font-size: 24px;
  }
  .success_msg{
    margin-top: 10px;
  }
</style>
