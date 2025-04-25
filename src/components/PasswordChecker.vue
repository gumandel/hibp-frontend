<template>
  <div class="password-checker">
    <div class="checker-card">
      <h2>Verifique a senha</h2>
      <p>Digite uma senha para verificar se foi exposta em vazamentos</p>
      
      <form @submit.prevent="checkPassword" class="checker-form">
        <input
          type="password"
          v-model="password"
          placeholder="Digite a senha"
          required
          class="password-input"
        >
        <button type="submit" :disabled="isLoading" class="check-button">
          {{ isLoading ? 'Verificando...' : 'Verificar' }}
        </button>
      </form>
      
      <div v-if="error" class="error-message">
        {{ error }}
      </div>
    </div>
    
    <div v-if="results" class="results-container">
      <div v-if="results.breaches && results.breaches.length > 0" class="pwned-results">
        <h3>⚠️ Resultados para "{{ password }}"</h3>
        <p>Esta senha foi encontrada em <strong>{{ results.breaches.length }}</strong> vazamentos conhecidos:</p>
        
        <div class="breach-list">
          <div v-for="breach in results.breaches" :key="breach.Name" class="breach-item">
            <h4>{{ breach.Title }}</h4>
            <p><strong>Data do vazamento:</strong> {{ formatDate(breach.BreachDate) }}</p>
            <p><strong>Dados comprometidos:</strong> {{ breach.DataClasses.join(', ') }}</p>
            <a :href="breach.Domain" target="_blank" rel="noopener" v-if="breach.Domain">Visitar site</a>
          </div>
        </div>
      </div>
      
      <div v-else-if="results" class="safe-result">
        <h3>✅ Nenhum vazamento encontrado</h3>
        <p>A senha "{{ password }}" não foi encontrada em nossos registros de vazamentos conhecidos.</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'PasswordChecker',
  data() {
    return {
      password: '',
      isLoading: false,
      error: null,
      results: null
    }
  },
  methods: {
    async checkPassword() {
      this.isLoading = true
      this.error = null
      this.results = null
      
      try {
        // Substitua pela sua chave de API real
        const apiKey = 'sua-chave-api-hibp'
        
        const response = await axios.get(
          `https://haveibeenpwned.com/api/v3/breachedaccount/${encodeURIComponent(this.password)}`,
          {
            headers: {
              'hibp-api-key': apiKey,
              'Accept': 'application/vnd.haveibeenpwned.v3+json'
            },
            params: {
              truncateResponse: false
            }
          }
        )
        
        this.results = {
          password: this.password,
          breaches: response.data
        }
      } catch (error) {
        if (error.response && error.response.status === 404) {
          this.results = {
            password: this.password,
            breaches: []
          }
        } else {
          this.error = error.response?.data || error.message || 'Erro ao verificar o e-mail'
          console.error('Erro na verificação:', error)
        }
      } finally {
        this.isLoading = false
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString('pt-BR')
    }
  }
}
</script>

<style scoped>
.password-checker {
  margin-bottom: 2rem;
}

.checker-card {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 2rem;
}

.checker-form {
  display: flex;
  gap: 10px;
  margin: 1.5rem 0;
}

.password-input {
  flex: 1;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.check-button {
  padding: 12px 24px;
  background-color: var(--primary-color);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.check-button:hover:not(:disabled) {
  background-color: var(--secondary-color);
}

.check-button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

.error-message {
  color: var(--error-color);
  padding: 10px;
  background-color: #fdecea;
  border-radius: 4px;
  margin-top: 1rem;
}

.results-container {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.pwned-results h3 {
  color: var(--error-color);
}

.safe-result h3 {
  color: var(--success-color);
}

.breach-list {
  margin-top: 1.5rem;
}

.breach-item {
  padding: 1rem;
  margin-bottom: 1rem;
  border-left: 4px solid var(--error-color);
  background-color: #f9f9f9;
}

.breach-item h4 {
  margin-top: 0;
  color: var(--primary-color);
}
</style>