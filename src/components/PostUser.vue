<template>
  <div>
    <form @submit.prevent="sendPostRequest" style="grid-gap: 20px; display: flex; flex-direction: column; align-items: center;">
      <input v-model="data.inn" type="text" placeholder="Введите ИНН" />
      <input v-model="data.requestDate" type="date" placeholder="Введите дату (формата 2012-01-25)" style="width: 222px"/>
      <button type="submit">Проверить</button>
    </form>
    <div v-if="response">
      <h2>Ответ:</h2>
      <pre>{{ response }}</pre>
    </div>
    <div v-if="error" style="color: red;">
      <h2>Ошибка:</h2>
      <pre>{{ error }}</pre>
    </div>
    <div v-if="trues" style="color: green;">
      <h2>Сообщение:</h2>
      <pre>{{ trues }}</pre>
    </div>
  </div>
</template>

<script>
const newDate = new Date();
const day = String(newDate.getDate()).padStart(2, '0');  // добавляем ведущий ноль, если день < 10
const month = String(newDate.getMonth() + 1).padStart(2, '0');  // месяцы от 0 до 11, поэтому +1
const year = newDate.getFullYear();
const formattedDate = `${year}-${month}-${day}`;
console.log(formattedDate);
import axios from 'axios';

export default {
  data() {
    return {
      data: {
        inn: '',
        requestDate: ''
      },
      response: null,
      error: null,
      trues: null
    };
  },
  methods: {
    async sendPostRequest() {
      try {
        const res = await axios.post('https://statusnpd.nalog.ru/api/v1/tracker/taxpayer_status', this.data);
        if (res.status === 200) {
          if (res.data.code === 'validation.failed') {
            this.error = res.data.message;
            this.response = null;
            this.trues = null;
          } else if (res.data.status === false) {
            this.error = res.data.message;
            this.response = null;
            this.trues = null;
          } else if (res.data.status === true) {
            this.trues = res.data.message;
            this.response = null;
            this.error = null;
          } else {
            this.response = res.data;
            this.error = null;
            this.trues = null;
          }
        }
      } catch (error) {
        if (error.response) {
          this.error = `Ошибка: ${error.response.data.message || 'Неизвестная ошибка'}`;
        } else if (error.request) {
          this.error = 'Нет ответа от сервера';
        } else {
          this.error = error.message;
        }
        this.response = null;
        this.trues = null; 
      }
    }
  }
};
</script>
