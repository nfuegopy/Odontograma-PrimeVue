<template>
  <header class="header">
    <h1>Aplicación de Odontograma</h1>
    <h2>Dra. Lusmela Anahi Melgarejo</h2>
  </header>
</template>

<script>
export default {
  name: 'AppHeader'
};
</script>

<style scoped>
.header {
  background-color: #007ad9;
  color: white;
  padding: 20px;
  text-align: center;
}

.header h1, .header h2 {
  margin: 0;
  font-weight: normal;
}

.header h1 {
  font-size: 2rem;
}

.header h2 {
  font-size: 1rem;
}
</style>
