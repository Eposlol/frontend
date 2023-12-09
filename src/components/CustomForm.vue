<template>
  <form class="form" @submit.prevent="handleSubmit()">
    <div class="form__item">
      <label>Email*:</label>
      <input type="text" name="email" placeholder="example@mail.com"  v-model="email">
    </div>
    <div class="form__item">
      <label>Phone-number:</label>
      <input type="text" name="phone" placeholder="99-99-99"  v-model="phone">
    </div>
    <button class="form__btn" type="submit">Отправить</button>
  </form>
  <ul class="result" v-html="result"></ul>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CustomForm',
  data() {
    return {
      email: '',
      phone: '',
      result: '',
    }
  },
  methods: {
    handleSubmit: async function() {
      let data = {
        email: this.email,
        phone: this.phone
      }
    
      let config = {
          header : {
          'Content-Type' : 'multipart/form-data'
        }
      }

     await axios.post('http://localhost:5000/getData', data, config)
     .then(
        response => {
          console.log(response.data)
          let list = '';
          [...response.data].forEach(element => {
              let {email, number} = element;
              list += `<li> <p>Email: ${email} <br> Number: ${number}</p> </li>`;
          });
          this.result = list;
        }
    )
    }
  }
}
</script>

<style>
  .form {
    display: flex;
    flex-direction: column;
    gap: 20px;
    max-width: 600px;
    margin: 0 auto;
  }

  .form__item {
    display: flex;
    flex-direction: column;
    text-align: start;
    gap: 5px;
  }

  .form__item label {
    color: #fff;
    font-size: 12px;
  }

  .form__item input {
    height: 40px;
    border-radius: 3px;
    outline: none;
    border: none;
    padding: 0 20px;
  }

  .form__btn {
    height: 40px;
    border-radius: 3px;
    border: none;
    transition: background .3s ease;
  }

  .form__btn:hover {
    background: #fff;
  }

  .result {
    display: flex;
    flex-direction: column;
    padding: 0;
    max-width: 600px;
    margin: 0 auto;
  }

  .result li {
    list-style: none;
    text-align: start;
  }
</style>