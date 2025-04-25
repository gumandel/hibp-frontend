<template>
  <div class="app">
    <header class="header">
      <h1>Verificador de Senhas Comprometidas</h1>
      <p>Integração com a API Have I Been Pwned</p>
    </header>
    
    <main class="main-content">
      <PasswordChecker />
    </main>
    
    <footer class="footer">
      <p>Backend: FastAPI | Frontend: Vue.js</p>
    </footer>
  </div>
</template>

<script>
import PasswordChecker from './components/PasswordChecker.vue'

export default {
  name: 'App',
  components: {
    PasswordChecker
  }
}
</script>

<style>
:root {
  --primary-color: #2c3e50;
  --secondary-color: #3498db;
  --background-color: #f5f7fa;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 0;
  padding: 0;
  background-color: var(--background-color);
  color: #333;
  line-height: 1.6;
}

.app {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  text-align: center;
  padding: 2rem;
  margin: 0 auto 2rem auto; /* Centraliza horizontalmente */
  background-color: var(--primary-color);
  color: white;
  border-radius: 8px;
  max-width: 650px; /* Mesma largura do formulário */
  width: calc(100% - 40px); /* Garante responsividade */
  box-sizing: border-box; /* Inclui padding na largura total */
}

.main-content {
  flex: 1;
}

.footer {
  text-align: center;
  padding: 1rem;
  margin-top: 2rem;
  color: #666;
  font-size: 0.9rem;
}
</style>