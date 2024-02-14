<template>
  <div class="container">
    <div class="content">
      <img alt="Vue logo" src="./assets/logo.png" class="logo">
      <EmprestimoForm/>
    </div>
  </div>
</template>

<script>
import EmprestimoForm from './components/EmprestimoForm.vue';

export default {
  name: 'App',
  components: {
    EmprestimoForm
  }
}
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 130vh; /* Ajuste conforme necessário */
}

.content {
  text-align: center;
}

.logo {
  margin-bottom: 1px; /* Espaçamento entre a imagem e o formulário */
}
</style>
