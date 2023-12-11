<template>
  <form class="form" @submit.prevent="handleSubmit">
    <div class="form__item" ref="email">
      <label>Email*:</label>
      <span class="form__error">Email введен неверно</span>
      <input type="text" name="email" placeholder="example@mail.com"  v-model="email">
    </div>
    <div class="form__item" ref="phone">
      <label>Номер телефона:</label>
      <span class="form__error">Телефон введен неверно</span>
      <input type="text" name="phone" placeholder="99-99-99" v-mask="'##-##-##'" v-model="phone">
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
      validate: false
    }
  },
  methods: {
    handleSubmit: async function() {

      this.formValidate();
      if(!this.validate) return;

      let data = {
        email: this.email.toLowerCase(),
        phone: this.phone
      }
    
      let config = {
          header : {
          'Content-Type' : 'multipart/form-data'
        }
      }

      await axios.post('http://localhost:3205/getData', data, config)
      .then(
        response => {
          let list = '';
          if(!response.data.length) {
            list += '<h2>Ничего не найдено</h2>';
          } else {
            list += '<h2>Результат поиска:</h2>';
            response.data.forEach(element => {
              let {email, number} = element;
              list += `<li> <p>Email: ${email} <br> Number: ${number}</p> </li>`;
            });
          }
          this.result = list;
        }
      )
      .catch(error => {
          console.log(error)
      })
    },
    formValidate: function() {
      this.phone = this.phone.replaceAll('-', '');
      this.$refs.email.classList.remove('error');
      this.$refs.phone.classList.remove('error');
      this.validate = true;

      if(!this.emailValidate(this.email)) { 
        this.$refs.email.classList.add('error');
        this.validate = false;
      }

      if (this.phone.length < 6 && this.phone.length >= 1){
        this.$refs.phone.classList.add('error');
        this.validate = false;
      }
    },
    emailValidate: function(email){
      return String(email)
        .toLowerCase()
        .match(
          /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        );
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
    color: #435585;
    font-size: 12px;
  }

  .form__item input {
    height: 40px;
    border-radius: 3px;
    outline: none;
    border: none;
    padding: 0 20px;
  }

  .form__item input {
    height: 40px;
    border-radius: 3px;
    outline: none;
    border: none;
    padding: 0 20px;
    border: 1px solid #fff;
  }
  
  .form__item .form__error {
    display: none;
    color: red;
    font-size: 12px;
  }

  .form__item.error .form__error {
    display: block;
  }

  .form__item.error input {
    border-color: red;
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