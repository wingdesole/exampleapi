<template>
  <v-container>
    <v-layout
      text-xs-center
      wrap
    >
      <v-flex xs12>
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        ></v-img>
        {{posts}}
      </v-flex>
      <v-form v-model="valid">
    <v-text-field
      v-model="username"
      :rules="usernameRules"
      :counter="10"
      label="username"
      required
    ></v-text-field>
    <v-text-field
      v-model="password"
      :rules="passwordRules"
      label="Password"
      required
    ></v-text-field>
  
  </v-form>

      <v-btn @click="pushBtn"> Register</v-btn>
      <v-btn @click="loginBtn"> Login </v-btn>
      <v-btn @click="testBtn"> Test </v-btn>

    </v-layout>
  </v-container>
</template>

<script>

import HTTP from './../services/Api'

  export default {
    data: () => ({
      valid: false,
      username: null,
      usernameRules: [
        v => !!v || 'username is required',
        v => v.length <= 10 || 'Name must be less than 10 characters'
      ],
      password : null,
      passwordRules: [
        v => !!v || 'Password is required',
      ],
      posts:'',
    }),
    methods : {
      testBtn() {
        HTTP().get('transactions/test')
        .then(res=>{
          console.log(res.data.message)
        })
      },
      pushBtn() {
        HTTP().post('users',{
          username:this.username,
          password:this.password,
          type: String
        })
        .then(res=>{
          console.log(res)
        })
        .catch(err=>{
          console.log(err)
        })
      },
      loginBtn(){
        HTTP().post('auth',{
          username:this.username,
          password:this.password,
          type: String
        })
        .then(res=>{
          const token = res.data;
          window.localStorage.setItem('x-auth-token', token);
        })
        .catch(err=>{
          console.log(err)
        })
      }
    }
  }
</script>

<style>

</style>
