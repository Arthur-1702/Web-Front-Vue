<template>
  <API_Correios/>
  <div style="display: flex; justify-content: center;">
    <div style="border: 1px solid #000; border-radius: 20px; padding: 10px;" v-if="showingDiv1">
        <h2 style="padding-left:20px ;"><u>CADASTRO</u></h2>
        <form @submit="createCadastro($event)">
            <label>Nome:</label> <br>
            <input type="text" v-model="nome"><br><br>

            <label>Email:</label> <br>
            <input type="text" v-model="email"><br><br>

            <label>Celular:</label> <br>
            <input type="text" v-model="telefone"><br><br>

            <input class="btn" type="submit" value="Cadastrar">
        </form>
    </div>
    <div v-if="showingDiv2">
        <h2>Editar cadastro</h2>
        <form @submit="editCadastro(cadastroSelecionado)">
            <label>Novo Nome</label><br>
            <input type="text" v-model="nome" placeholder="Para manter, deixe vazio"><br><br>

            <label>Novo Email</label><br>
            <input type="text" v-model="email" placeholder="Para manter, deixe vazio"><br><br>

            <label>Novo Celular</label><br>
            <input type="text" v-model="telefone" placeholder="Para manter, deixe vazio"><br><br>

            <input class="btn" type="submit" value="Editar">
            <button class="btn" @click="showDiv1" v-if="!showingDiv1">Voltar</button>
        </form>
        
    </div>
  </div>
    <hr>
        <div>
        <div id="cadastro-table-heading">
            <div class="cadastro-id">#:</div>
            <div>Nome:</div>
            <div>Email:</div>
            <div>Número:</div>
        </div>
        </div>
        <div id="cadastro-table-rows">
        <div class="cadastro-table-row" v-for="cadastro in cadastros" :key="cadastro.id">
            <div>{{ cadastro.nome }}</div>
            <div>{{ cadastro.email }}</div>
            <div>{{ cadastro.telefone }}</div>
            <div>
                <button class="btn" @click="deleteCadastro(cadastro.id)">Apagar</button>
            </div>
            <div>
                <button class="btn" @click="showDiv2(cadastro.id)" v-if="!showingDiv2"> Editar </button>
            </div>
        </div>
        </div>
</template>

<script>
  import API_Correios from "./API_Correios.vue" 
    
  export default{
    name: "FormCadastro",
    components: {
      API_Correios
    },
    data() {
        return{
            nome: null,
            email: null,
            telefone: null,
            cadastros: null,
            cadastros_id: null,
            showingDiv1: true,
            showingDiv2: false,
            cadastroSelecionado: null
        }
    },
    methods: {

      showDiv1() {
        this.showingDiv1 = true;
        this.showingDiv2 = false;
        this.cadastroSelecionado = null
      },
      showDiv2(id) {
        this.showingDiv1 = false;
        this.showingDiv2 = true;
        this.cadastroSelecionado = id;
      },
      async createCadastro(e){
            e.preventDefault(); 
            const data = {
                nome : this.nome,
                email : this.email,
                telefone : this.telefone
            }

            const dataJson = JSON.stringify(data);
            const req = await fetch('http://localhost:3000/cadastros', {
                method: 'POST',
                headers: {"Content-Type" : "application/json" },
                body: dataJson
            });

            const res = await req.json();
            console.log(res);
            
            this.getCadastros();
            
        },

        async getCadastros() {
        const req = await fetch('http://localhost:3000/cadastros');
       
        const data = await req.json();     
        this.cadastros = data;

        console.log(this.cadastros);
        
        },
        async getCadastro(id) {
        const req = await fetch(`http://localhost:3000/cadastros/${id}`);
       
        const data = await req.json();     
        this.cadastroSelecionado = data; 

        },
        async deleteCadastro(id) {
            const req = await fetch(`http://localhost:3000/cadastros/${id}`, {
            method: 'DELETE'
            });

            this.getCadastros();
        },
        async editCadastro(id) {
          const data = {
                nome : this.nome,
                email : this.email,
                telefone : this.telefone
            }

            const dataJson = JSON.stringify(data);
            const req = await fetch(`http://localhost:3000/cadastros/${id}`, {
                method: 'PATCH',
                headers: {"Content-Type" : "application/json" },
                body: dataJson
            });

            this.getCadastros();
        }

    },
    mounted() {
        this.getCadastros();
    }
    }

</script>

<style scoped>
  #cadastro-table {
    max-width: 1200px;
    margin: 0 auto;
  }
  #cadastro-table-heading,
  #cadastro-table-rows,
  .cadastro-table-row {
    display: flex;
    flex-wrap: wrap;
  }
  #cadastro-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }
  .cadastro-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }
  #cadastro-table-heading div,
  .cadastro-table-row div {
    width: 19%;
  }
  #cadastro-table-heading .cadastro-id,
  .cadastro-table-row .cadastro-number {
    width: 5%;
  }
  select {
    padding: 12px 6px;
    margin-right: 12px;
  }
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