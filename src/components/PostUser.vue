<template>
  <div>
    <form @submit.prevent="sendPostRequest" style="grid-gap: 20px; display: flex; flex-direction: column; align-items: center;">
      <input v-model="data.inn" type="text" placeholder="Введите ИНН" />
      <input v-model="data.requestDate" type="date" placeholder="Введите дату (формата 2012-01-25)" style="width: 222px"/>
      <button type="submit">Проверить</button>
    </form>
    <div v-if="error" class="answer" style="color: red">
      <pre class="pre">{{ error }}</pre>
    </div>
    <div v-if="trues" class="answer" style="color: green">
      <pre class="pre">{{ trues }}</pre>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      data: {
        inn: '',
        requestDate: ''
      },
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
            this.trues = null;
          } else if (res.data.status === false) {
            this.error = res.data.message;
            this.trues = null;
          } else if (res.data.status === true) {
            this.trues = res.data.message;
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
        this.trues = null; 
      }
    }
  }
};
</script>

<style scoped>
.answer {
  margin-top: 15px;
  background-color: #f4f4f4;
  border: 1px solid #ddd;
  font-family: monospace;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.pre{
  padding: 10px 3px 10px 10px;
  white-space: pre-wrap;
  margin: 0px;
}
</style>
