<template>
  <div class="checker-card vl-parent">
    <loading
      v-model:active="isLoading"
      :can-cancel="false"
      :is-full-page="false"
      background-color="rgba(15, 23, 42, 0.12)"
      color="#2563eb"
      loader="dots"
    />

    <form class="checker-form" @submit.prevent="sendPostRequest">
      <div class="field">
        <label for="inn">ИНН</label>
        <input
          id="inn"
          v-model.trim="data.inn"
          type="text"
          inputmode="numeric"
          placeholder="Введите ИНН"
        />
      </div>

      <div class="field field-date">
        <label for="requestDate">Дата установки статуса</label>
        <input
          id="requestDate"
          v-model="data.requestDate"
          type="date"
        />
      </div>

      <button class="submit-btn" type="submit" :disabled="isLoading">
        {{ isLoading ? 'Проверяем...' : 'Проверить' }}
      </button>
    </form>

    <div
      v-if="answer"
      class="result-box"
      :class="responseStatus"
      role="status"
      aria-live="polite"
    >
      <div class="result-head">
        <span class="status-dot"></span>
        <strong>{{ responseStatus === 'success' ? 'Результат проверки' : 'Ошибка проверки' }}</strong>
      </div>
      <p class="result-text">{{ answer }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Loading from 'vue-loading-overlay'
import 'vue-loading-overlay/dist/css/index.css'
import { nextTick } from 'vue'

export default {
  components: {
    Loading
  },
  data() {
    return {
      isLoading: false,
      data: {
        inn: '',
        requestDate: new Date().toISOString().split('T')[0]
      },
      responseStatus: 'error',
      answer: null
    }
  },
  methods: {
    async sendPostRequest() {
      this.isLoading = true
      await nextTick()
      this.answer = null

      try {
        const res = await axios.post(
          'https://statusnpd.nalog.ru/api/v1/tracker/taxpayer_status',
          this.data
        )

        this.answer = res.data.message
        this.responseStatus = res.data.status === false ? 'error' : 'success'
      } catch (error) {
        this.responseStatus = 'error'

        if (error.response?.data?.code === 'validation.failed') {
          this.answer = 'Ошибка: указан некорректный ИНН'
        } else if (error.response) {
          this.answer = `Ошибка: ${error.response.data.message || 'Неизвестная ошибка'}`
        } else if (error.request) {
          this.answer = 'Нет ответа от сервера'
        } else {
          this.answer = error.message
        }
      } finally {
        this.isLoading = false
      }
    }
  }
}
</script>

<style scoped>
.checker-card {
  position: relative;
}

.checker-form {
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.field {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 8px;
  text-align: left;
}

.field label {
  font-size: 14px;
  font-weight: 600;
  color: #475569;
}

.field input {
  width: 100%;
  min-height: 50px;
  padding: 0 16px;
  border: 1px solid #dbe2ea;
  border-radius: 14px;
  background: #ffffff;
  color: #1e293b;
  font-size: 15px;
  outline: none;
  transition: border-color 0.2s ease, box-shadow 0.2s ease, background-color 0.2s ease;
}

.field input:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.14);
}

.field input::placeholder {
  color: #94a3b8;
}

.submit-btn {
  min-height: 52px;
  margin-bottom: 15px;
  border: none;
  border-radius: 14px;
  background: #2f6fed;
  color: #ffffff;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
  transition: background-color 0.2s ease, transform 0.15s ease, box-shadow 0.2s ease;
  box-shadow: 0 10px 24px rgba(47, 111, 237, 0.18);
}

.submit-btn:hover {
  background: #1f5fe0;
  transform: translateY(-1px);
}

.submit-btn:disabled {
  opacity: 0.7;
  cursor: wait;
  transform: none;
}

.result-box {
  margin-top: 18px;
  border-radius: 16px;
  padding: 16px 18px;
  border: 1px solid #e2e8f0;
  background: #f8fafc;
}

.result-box.success {
  background: #f0fdf4;
  border-color: #bbf7d0;
}

.result-box.error {
  background: #fff7ed;
  border-color: #fed7aa;
}

.result-head {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
  color: #0f172a;
}

.status-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  flex-shrink: 0;
}

.result-box.success .status-dot {
  background: #16a34a;
}

.result-box.error .status-dot {
  background: #ea580c;
}

.result-text {
  margin: 0;
  color: #475569;
  line-height: 1.55;
  white-space: pre-wrap;
  word-break: break-word;
}
</style>