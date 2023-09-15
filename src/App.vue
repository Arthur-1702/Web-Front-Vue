<template>
  <div>
    <Header/>
    <Titulo/>
    <FormCadastro/>
  </div>
</template>

<script>
import FormCadastro from "./components/FormCadastro.vue";
import Header from "./components/Header.vue";
import Titulo from "./components/Titulo.vue";


  export default{
    name: 'App',
    components: {
      Titulo,
      FormCadastro,
      Header
    }
  }
</script>

<style>
  body{
    background-color: rgb(60, 60, 167);
    color: #FFF;
  }
</style>

