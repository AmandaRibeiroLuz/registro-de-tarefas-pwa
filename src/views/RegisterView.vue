<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import authApi from '../api/authApi'

const router = useRouter()

const email = ref('')
const password = ref('')
const confirmPassword = ref('')
const error = ref('')
const loading = ref(false)

async function register() {
  error.value = ''

  if (password.value !== confirmPassword.value) {
    error.value = 'As senhas não coincidem.'
    return
  }

  loading.value = true

  try {
    await authApi.register(email.value, password.value)

    router.push('/login?registered=true')
  } catch (err) {
    if (err.response?.status === 400) {
      error.value = err.response.data.detail
    } else {
      error.value = 'Erro ao cadastrar usuário.'
    }
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="register">
    <h1>Criar Conta</h1>

    <form @submit.prevent="register">
      <input
        v-model="email"
        type="email"
        placeholder="Email"
        required
      />

      <input
        v-model="password"
        type="password"
        placeholder="Senha"
        required
      />

      <input
        v-model="confirmPassword"
        type="password"
        placeholder="Confirmar senha"
        required
      />

      <p v-if="error">
        {{ error }}
      </p>

      <button :disabled="loading">
        {{ loading ? 'Cadastrando...' : 'Cadastrar' }}
      </button>
    </form>
  </div>
</template>
<style scoped>
.register {
  justify-content: center;
  align-items: center;
  margin-top: 50px;
}

.register h1 {
  margin-bottom: 30px;
  color: #fe874c;
  font-size: 1.8rem;
  text-align: center;
}

.register input {
  width: 100%;
  box-sizing: border-box;
  padding: 12px;
  margin-bottom: 20px;
  border: 2px solid #a5a4a4;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
}

.register input:focus {
  border-color: #fe874c;
}

.register p {
  margin: 0 0 16px;
  padding: 10px 12px;
  background: #fff1ec;
  border-radius: 6px;
  color: #d9534f;
  font-size: 0.875rem;
}

.register button {
  width: 100%;
  padding: 12px 25px;
  background-color: #fe874c;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
}

.register button:hover {
  background-color: #d75a25;
}

</style>
