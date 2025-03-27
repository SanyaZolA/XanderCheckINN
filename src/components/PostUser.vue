<template>
  <loading v-model:active="isLoading" :can-cancel="true" @cancel="onCancel" background-color="#98928d" loader="dots" />
  <div>
    <form @submit.prevent="sendPostRequest" style="grid-gap: 20px; display: flex; flex-direction: column; align-items: center;">
      <input v-model="data.inn" type="text" placeholder="Введите ИНН" />
      <input v-model="data.requestDate" type="date" style="width: 125px"/>
      <button type="submit">Проверить</button>
    </form>
    <div v-if="error" class="answer" style="color: red">
      <pre class="pre">{{ error }}</pre>
    </div>
    <div v-if="answerINN" class="answer" style="color: green">
      <pre class="pre">{{ answerINN }}</pre>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/css/index.css';
import { nextTick } from 'vue';

export default {
  data() {
    return {
      isLoading: false,
      fullPage: true,
      data: {
        inn: '',
        requestDate: new Date().toISOString().split('T')[0],
      },
      error: null,
      true: null
    }
  },
      components: {
            Loading
        },
  methods: {
    async sendPostRequest() {
      this.isLoading = true;
      await nextTick();
      this.error = null;
      try {
        const res = await axios.post('https://statusnpd.nalog.ru/api/v1/tracker/taxpayer_status', this.data);
        if (res.status === 200) {
          if (res.data.code === 'validation.failed') {
            this.error = res.data.message;
            this.answerINN = null;
          } else if (res.data.status === false) {
            this.error = res.data.message;
            this.answerINN = null;
          } else if (res.data.status === true) {
            this.answerINN = res.data.message;
            this.error = null;
          } else {
            this.response = res.data;
            this.error = null;
            this.answerINN = null;
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
        this.answerINN = null; 
      } finally {
        this.isLoading = false;
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
