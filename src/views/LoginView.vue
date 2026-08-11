<script setup>
import { ref } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import { useAuthStore } from '../stores/auth';

const router = useRouter();
const route = useRoute();
const authStore = useAuthStore();

const email = ref('');
const password = ref('');
const loading = ref(false);
const errorMessage = ref('');

async function handleLogin() {
  loading.value = true;
  errorMessage.value = '';
  try {
    await authStore.login(email.value, password.value);
    router.push('/');
  } catch (err) {
    errorMessage.value =
      err.response?.data?.detail ??
      'Erro ao entrar. Verifique suas credenciais.';
  } finally {
    loading.value = false;
  }
}
</script>
<template>
  <div class="login-container">
    <form class="login-form" @submit.prevent="handleLogin">
      <p v-if="route.query.registered === 'true'">
        Cadastro realizado com sucesso! Faça login.
      </p>

      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>

      <h1>Login</h1>
      <div class="field">
        <label for="email">Email</label>
        <input id="email" v-model="email" type="email" placeholder="seu@email.com" required autocomplete="email" />
      </div>

      <div class="field">
        <label for="password">Senha</label>
        <input id="password" v-model="password" type="password" placeholder="••••••••" required
          autocomplete="current-password" />
      </div>

      <button type="submit" :disabled="loading">
        {{ loading ? 'Entrando...' : 'Entrar' }}
      </button>
      <RouterLink to="/register">
        Criar conta
      </RouterLink>
    </form>
  </div>
</template>
<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 50px;
}

.login-form {
  width: 100%;
  max-width: 420px;
}

.login-form p {
  margin-bottom: 20px;
  padding: 10px 12px;
  background-color: #eef7ee;
  border-radius: 8px;
  color: #4d8b4d;
  font-size: 0.9rem;
}

.login-form h1 {
  margin-bottom: 30px;
  color: #fe874c;
  font-size: 1.8rem;
  text-align: center;
}

.error-message {
  margin-bottom: 20px;
  padding: 10px 12px;
  background-color: #fff1ec;
  border-radius: 8px;
  color: #d9534f;
  font-size: 0.9rem;
}

.field {
  margin-bottom: 20px;
}

.field label {
  display: block;
  margin-bottom: 6px;
  color: #555;
  font-size: 0.9rem;
  font-weight: 500;
}

.field input {
  width: 100%;
  box-sizing: border-box;
  padding: 12px;
  border: 2px solid #a5a4a4;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
}

.field input:focus {
  border-color: #fe874c;
}

.field input::placeholder {
  color: #aaa;
}

.login-form button {
  width: 100%;
  padding: 12px 25px;
  margin-bottom: 16px;
  background-color: #fe874c;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
}

.login-form button:hover:not(:disabled) {
  background-color: #d75a25;
}

.login-form button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.login-form a {
  display: block;
  text-align: center;
  color: #fe874c;
  font-size: 0.9rem;
  font-weight: 500;
  text-decoration: none;
}

.login-form a:hover {
  text-decoration: underline;
}
</style>
