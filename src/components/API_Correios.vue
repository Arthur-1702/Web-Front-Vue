<template>
    <div>
        <h3></h3>
        <input style="margin-right: 20px ;" type="text" v-model="cep" placeholder="CEP" />
        <button class="btn" @click="buscarEndereco">Buscar</button>
        <div v-if="endereco">
            <p>{{ endereco.logradouro }}, {{ endereco.bairro }}, {{ endereco.localidade }} - {{ endereco.uf }}</p>
        </div>
    </div>
</template>
  
<script>
    import axios from 'axios';
  
    export default {
    data() {
        return {
            cep: '',
            endereco: null,
        };
    },
    methods: {
        buscarEndereco() {
            axios.get(`https://viacep.com.br/ws/${this.cep}/json/`).then((response) => {
                this.endereco = response.data;
            })
            .catch((error) => {
                console.log(error);
            });
        },
    }
}
</script>

<style scoped>
.btn {
    background-color: #031B4E;
    color:#fff;
    font-weight: bold;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
    margin-right: 10px;
    border-radius: 15px;
  }
  
  .btn:hover {
    background-color: transparent;
    color: #222;
  }
  
</style>