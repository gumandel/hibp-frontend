<template>
  <div class="app">
    <header class="header">
      <h1>Verificador de Vazamentos</h1>
      <p>Descubra se uma senha foi comprometida em vazamentos de dados</p>
    </header>
    
    <main class="main-content">
      <PasswordChecker />
    </main>
    
    <footer class="footer">
      <p>Inspirado no serviço "Have I Been Pwned"</p>
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
/* Estilos básicos */
:root {
  --primary-color: #2c3e50;
  --secondary-color: #1a2b3c;
  --background-color: #f5f7fa;
  --text-color: #333;
  --error-color: #e74c3c;
  --success-color: #2ecc71;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  line-height: 1.6;
  margin: 0;
  padding: 0;
  background-color: var(--background-color);
  color: var(--text-color);
}

.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  text-align: center;
  padding: 2rem 0;
  margin-bottom: 2rem;
  background-color: var(--primary-color);
  color: white;
  border-radius: 8px;
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