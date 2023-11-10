<!--Login.vue-->
<template>
  <div class="LogIn">
    <h1>登入</h1>
    <form @submit.prevent="signIn()">
      <ul>
        <li>
          <label for="account">帳號</label>
          <input type="text" id="account" v-model="userName" required />
        </li>
        <li>
          <label for="password">密碼</label>
          <input type="password" id="password" v-model="password" required />
        </li>
        <li class="button">
          <button type="submit">登入</button>
        </li>
      </ul>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      userName: '',
      password: ''
    }
  },
  methods: {
    signIn() {
      this.$router.push('/home')
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
form {
/* Center the form on the page */
margin: 0 auto;
width: 400px;
/* Form outline */
padding: 1em;
border: 1px solid #CCC;
border-radius: 1em;
}
ul {
list-style: none;
padding: 0;
margin: 0;
}
form li + li {
margin-top: 1em;
}
label {
/* Uniform size & alignment */
/*display: inline-block;*/
width: 90px;
text-align: right;
padding-right: 20px;
}
input {
/* To make sure that all text fields have the same font settings
By default, textareas have a monospace font */
font: 1em sans-serif;
/* Uniform text field size */
width: 300px;
height: 30px;
box-sizing: border-box;
/* Match form field borders */
border: 1px solid #999;
}
input:focus {
/* Additional highlight for focused elements */
border-color: #f00;
background-color: rgb(207, 248, 241);
}
.button {
/* Align buttons with the text fields */
padding-left: 0px; /* same size as the label elements */
}

button {
/* This extra margin represent roughly the same space as the space
between the labels and their text fields */
color: #fff;
background-color: #000;
margin-left: .5em;
width:50%;
}
</style>