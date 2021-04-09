<template>
    <div>
        <table  class ="table">
            <thead>
                <tr class ="table table-dark"> 

                    <th scope="col">ID</th>          
                    <th scope="col">FEITO</th>
                    
                    <th scope="col">LAVA PRATO</th>
                    <th scope="col">VARRE CASA</th>
                    <th scope="col">BANHEIRO</th>
                    <th scope="col">DATA</th>
                    <th colspan="1"></th>

                </tr>
            </thead>
            <tbody>
                <tr v-for="pessoa in pessoas" :key="pessoa.id" >
                    <td>{{pessoa.id}}</td>
                        <th scope="row"> 
                         <input type="checkbox" id="checkbox" :checked="pessoa.feito"  disabled/>
                     </th> 
         
                    <td>{{ pessoa.lavPrato }}</td>
                    <td>{{ pessoa.varreC }}</td>
                    <td>{{ pessoa.banheiro }}</td>
                    <td>{{ formatData(pessoa.dataTask) }}</td>

                    <td>
                        <button class = "btn btn-danger" @click="editar(pessoa)">Editar</button>
                    </td>
     
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: "Lista",
    props: {
        pessoas: Array,
    },

    methods: {
        formatData(data) {
            return new Date(data).toLocaleDateString()
        },

        editar(pessoa) {
       
            this.inputLavarP = pessoa.lavPrato 
            this.inputVarreC = pessoa.varreC
            this.inputBanheiro = pessoa.banheiro        
            this.inputData = new Date(pessoa.dataTask).toISOString().split('T')[0]
            this.inputFeito = pessoa.feito
            
            document.getElementById("id").value=pessoa.id  
            this.pessoa = pessoa
            this.editMode = true
        },
    }

    
}
</script>