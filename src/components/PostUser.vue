<template>
  <loading v-model:active="isLoading" :can-cancel="true" background-color="#98928d" loader="dots" />
  <div>
    <form @submit.prevent="sendPostRequest" style="grid-gap: 20px; display: flex; flex-direction: column; align-items: center;">
      <input v-model="data.inn" type="text" placeholder="Введите ИНН" />
      <input v-model="data.requestDate" type="date" style="width: 125px"/>
      <button type="submit">Проверить</button>
    </form>
    <div v-if="answer" class="frame" :style="{ color: responseColor }">
      <pre class="text">{{ answer }}</pre>
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
      data: {
        requestDate: new Date().toISOString().split('T')[0],
      },
      responseColor: 'red',
      answer: null
    }
  },
      components: {
            Loading
        },
  methods: {
    async sendPostRequest() {
      this.isLoading = true;
      await nextTick();
      this.answer = null;
      try {
        const res = await axios.post('https://statusnpd.nalog.ru/api/v1/tracker/taxpayer_status', this.data);
        if (res.data.status === false) {
          this.answer = res.data.message;
          this.responseColor = 'red';
        } else {
          this.answer = res.data.message;
          this.responseColor = 'green';
      }
      } catch (error) {
        if (error.response) {
          this.responseColor = 'red';
          this.answer = `Ошибка: ${error.response.data.message || 'Неизвестная ошибка'}`;
        } else if (error.request) {
          this.answer = 'Нет ответа от сервера';
        } else {
          this.answer = error.message;
        }
      } finally {
        this.isLoading = false;
      }
    }
  }
};
</script>

<style scoped>
.frame {
  margin-top: 15px;
  background-color: #f4f4f4;
  border: 1px solid #ddd;
  font-family: monospace;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.text{
  padding: 10px 3px 10px 10px;
  white-space: pre-wrap;
  margin: 0px;
}
</style>
