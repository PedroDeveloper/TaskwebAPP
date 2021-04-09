<template>
<div class="container">
  <!-- <Header /> TODO: BREAK INTO SMALL COMPONENTS -->
  <h1 class="title">DailyTasks</h1>
  <div class="table-responsive-md">
    <table  class ="table table-hover">
        <thead>
          <tr class ="table table-dark"> 
            <th colspan="1"></th>
              <th scope="col">FEITO</th>
              <th scope="col">LAVA PRATO</th>
              <th scope="col">VARRER CASA</th>
              <th scope="col">BANHEIRO</th>
              <th scope="col">DATA</th>
              
          </tr>
        </thead>
        <tbody>
          <tr v-for="pessoa in pessoas" :key="pessoa.id" >

            <td>
                <button class = "btn btn-danger" @click="editar(pessoa)">Editar</button>
              </td>
              <th scope="row"> 

                <input type="checkbox" id="checkbox" :checked="pessoa.feito"  disabled/>

            
            </th> 
          
              <td>{{pessoa.lavPrato}}</td>
              <td>{{pessoa.varreC}}</td>
              <td>{{pessoa.banheiro}}</td>
              <td>{{ formatData(pessoa.dataTask) }}</td>

              
    
          </tr>
        </tbody>
      </table>
    </div>


  <form>

    <div class="form-row">

    <div class="form-group">
      <label for="id" hidden="true">id</label>
      <input type="int" class="form-control" id="id" disabled hidden="true">
    </div>

  <div class="input-group flex-nowrap">
      <!-- <label for="lavaP">Lavar o Prato (*)</label> -->
      <span class="input-group-text" id="addon-wrapping">Lavar o Prato *</span>
      <input type="text" class="form-control" id="lavaP" aria-describedby="addon-wrapping" v-model="inputLavarP">
    </div>
  

 <div class="input-group">
    <span class="input-group-text">Varrer a casa *</span>
    <input type="text" class="form-control" id="varreC" v-model="inputVarreC">
  </div>

  <div class="input-group">
    <span class="input-group-text">Banheiro</span>
    <input type="text" class="form-control" id="banheiro" v-model="inputBanheiro">
  </div>

  <div class="input-group">
      <span class="input-group-text">Data</span>
      <input type="date"  value="2017-06-01" class="form-control" id="data" v-model="inputData">
  </div>
   
  
   <div class="input-group">
      <span class="input-group-text">Feito (*)</span>
      <select id="feito" class="form-select" v-model="inputFeito">
        <option value="1" :selected="inputFeito === 1 ? true : false ">Concluido</option>
        <option value="0" >Não Concluido</option>
      </select>
    </div>  

     <br/>

  <div v-if="!editMode">
    <button @click="salvar" type="button" class="btn btn-primary">Salvar</button>
  </div>

  <div v-else>
    <button  @click="alterar" type="button" class="btn btn-danger">Salvar Alterações</button>
    <button @click="cancelEdit" type="button" class="btn btn-warning">Cancelar Alterações</button>
  </div>
</div>
</form> 
<br/>
<br/>
      
 


    <!-- <Lista :pessoas="pessoas" /> -->

</div>


    
</template>

<script>
import axios from 'axios'
// import Header from './Header'
// import Lista from './Lista'

export default {
  name: 'pessoas',

  // components: {
  //   Header,
  //   Lista
  // },

  data () {
    return {
      
      pessoas: [],
      pessoa:undefined,
      editMode: false,
      inputLavarP: '',
      inputVarreC : '',
      inputBanheiro: '',
      inputData: null,
      inputFeito: 0,

    }
  },
  
  methods: 
  {
    lista () {

      // 192.168.10.17/api/pessoa/dia return only the today task
      // 192.168.10.17/api/pessoa return all tasks
      axios.get(`http://taskcasa.azurewebsites.net/api/pessoa/dia`).then((res)=> {
        this.pessoas = res.data
        console.log(res.data)
        
      })
    },

    salvar () {

      if(!this.validatedInputData())
        alert("Insira os dados com *")
      else {
        axios.post(`http://taskcasa.azurewebsites.net/api/pessoa`, {
          lavPrato: this.inputLavarP,
          varreC: this.inputVarreC,
          banheiro:  this.inputBanheiro,
          dataTask:  this.inputData,
          feito:   this.inputFeito
        }).then(() => {
            this.lista()
            this.resetInput()
        })
      }
      
     
      
    },
    editar(pessoa){
       
      this.inputLavarP = pessoa.lavPrato 
      this.inputVarreC = pessoa.varreC
      this.inputBanheiro = pessoa.banheiro        
      this.inputData = new Date(pessoa.dataTask).toISOString().split('T')[0]
      this.inputFeito = pessoa.feito
      
      document.getElementById("id").value=pessoa.id  
      this.pessoa = pessoa
      this.editMode = true
    },

    alterar () {

      this.pessoa.lavPrato = this.inputLavarP
      this.pessoa.varreC =  this.inputVarreC
      this.pessoa.banheiro =  this.inputBanheiro
      this.pessoa.dataTask = this.inputData
      this.pessoa.feito = this.inputFeito
      this.pessoa.id = document.getElementById("id").value            

      

      // TODO: VALIDATE THE DATA FROM INPUT, ONLY MAKE REQUESSTS IF THE USER SET AN  VALUE TO THE FEITO FIELD
      if (!this.validatedInputData()) {
        alert("Por favor, insira os campos com *")
      } else {
          axios.put(`http://taskcasa.azurewebsites.net/api/pessoa`,this.pessoa).then(()=>{
            this.lista()            
              // alert("ENVIADO")
        })
        
        this.editMode = false
        this.resetInput()
      }

      
    },

    formatData(data) {
      return new Date(data).toLocaleDateString()
    },


    resetInput() {
      this.inputLavarP =  ''
      this.inputVarreC  =  ''
      this.inputBanheiro =  ''
      this.inputData =  null
      this.inputFeito =  0
    },

    cancelEdit() {
      this.editMode = false
      this.resetInput()
    },


    validatedInputData() {
      if ((this.inputLavarP.trim().length != 0 && this.inputVarreC.trim().length != 0) && typeof(this.inputFeito) != 'boolean')
        return true
      
      return false
    },

  }, 
  created() {
    this.lista()
  },
}

</script>


<style lang="scss">

  .title {
    text-align: center;
    padding: 20px;
  }
  
  .form-row {
    margin-top: 50px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }


  .btn {
    // display: flex;
    width: 5.9rem;
    margin: 0 10px;
    border-radius: 15px;
    // flex-direction: row;
  }


  .input-group {
    margin-bottom: 10px;
  }

  .input-group-text {
    width: 150px;
    display: flex;
    justify-content: center;
    background: linear-gradient(to right, rgba(0, 0, 0, 0.75), rgba(0, 0, 0, 0.5));
    
    font-weight: 700;
    color: #fff;
  }


  @media only screen and (min-width:768px) {
    .input-group {
      width: 30%;
    }

    .btn-warning {
      margin-left: 50px;
    }

    .btn {
      display: inline-block;
    }
  }


</style>