<template>
  <div class="password-checker">
    <h2>Verificação de Senha</h2>
    <p>Verifique se sua senha foi comprometida em vazamentos de dados</p>
    
    <form @submit.prevent="checkPassword" class="check-form">
      <input
        type="password"
        v-model="password"
        placeholder="Digite sua senha"
        required
        class="password-input"
      >
      <div class="button-group">
        <button
          type="submit"
          :disabled="isLoading"
          @click="checkType = 'breached'"
          class="check-button"
        >
          {{ isLoading && checkType === 'breached' ? 'Verificando...' : 'Verificar Comprometimento' }}
        </button>
        <button
          type="submit"
          :disabled="isLoading"
          @click="checkType = 'breach_qnt'"
          class="check-button secondary"
        >
          {{ isLoading && checkType === 'breach_qnt' ? 'Verificando...' : 'Verificar Quantidade' }}
        </button>
      </div>
    </form>
    
    <div v-if="error" class="error-message">
      {{ error }}
    </div>
    
    <div v-if="result !== null" class="result-container">
      <div v-if="checkType === 'breached'" class="result-section">
        <h3>Resultado:</h3>
        <p v-if="result === true" class="pwned">
          ⚠️ Esta senha foi comprometida!
        </p>
        <p v-else class="safe">
          ✅ Esta senha não foi encontrada em vazamentos conhecidos
        </p>
      </div>
      
      <div v-if="checkType === 'breach_qnt'" class="result-section">
        <h3>Resultado:</h3>
        <p v-if="result > 0" class="pwned">
          ⚠️ Esta senha foi encontrada {{ result }} vezes em vazamentos
        </p>
        <p v-else class="safe">
          ✅ Esta senha não foi encontrada em vazamentos conhecidos
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PasswordChecker',
  data() {
    return {
      password: '',
      isLoading: false,
      error: null,
      result: null,
      checkType: 'breached' // 'breached' ou 'breach_qnt'
    }
  },
  methods: {
    async checkPassword() {
      this.isLoading = true
      this.error = null
      this.result = null
      
      try {
        const endpoint = this.checkType === 'breached' 
          ? 'breached' 
          : 'breach_qnt'
        
        const response = await fetch(
          `http://localhost:8000/${endpoint}/${encodeURIComponent(this.password)}`
        )
        
        if (!response.ok) {
          throw new Error('Erro ao verificar senha')
        }
        
        this.result = await response.json()
      } catch (err) {
        this.error = err.message
        console.error('Erro:', err)
      } finally {
        this.isLoading = false
      }
    }
  }
}
</script>

<style scoped>
.password-checker {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #2c3e50;
  margin-top: 0;
  text-align: center;
}

.check-form {
  margin: 2rem 0;
}

.password-input {
  width: 100%;
  padding: 12px;
  margin-bottom: 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.button-group {
  display: flex;
  gap: 1rem;
}

.check-button {
  flex: 1;
  padding: 12px;
  background-color: #2c3e50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.check-button.secondary {
  background-color: #3498db;
}

.check-button:hover:not(:disabled) {
  opacity: 0.9;
}

.check-button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

.error-message {
  color: #e74c3c;
  padding: 10px;
  background-color: #fdecea;
  border-radius: 4px;
  margin-top: 1rem;
}

.result-container {
  margin-top: 2rem;
  padding: 1rem;
  border-radius: 4px;
  background-color: #f8f9fa;
}

.result-section {
  padding: 1rem;
}

.pwned {
  color: #e74c3c;
  font-weight: bold;
}

.safe {
  color: #27ae60;
  font-weight: bold;
}
</style>