<template>
  <div id="app">
    <h1>Pesquisa de Desenvolvedores</h1>
    <div class="cabecalho">
        <div>
            <label for="name">Por nome:</label>
            <input class="search" id="name" placeholder="Digite aqui">
        </div>
        <div class="filtros">
            <div class="opcao">
                <label>Por linguagem:</label>
                <input type="radio" name="opcao" value="e">E
                <input type="radio" name="opcao" value="ou">Ou
            </div>
            <div class="linguagens">
                <div>
                    <input type="checkbox" name="linguagem" value="java">Java
                    <input type="checkbox" name="linguagem" value="javascript">JavaScript
                </div>
                <div>
                    <input type="checkbox" name="linguagem" value="php">PHP
                    <input type="checkbox" name="linguagem" value="python">Python
                </div>
            </div>
        </div>
    </div>
    <div>
        <h4 v-text="devs.length + ' Resultados'"/>
    </div>
    
    <div class="listagem">
        <div class="card" v-for="dev in devs" :key="dev.key" @click="abreModal(dev)">
            <div>
                <img class="user-image" :src="dev.picture">
            </div>
            <div class="info">
                <span class="name" v-text="dev.name"/>
                <span class="age" v-text="dev.age + 'anos'"/>
                <span class="email" v-text="dev.email"/>
                <div>
                    <img class="languages" v-for="lang in dev.programmingLanguages" :key="lang.key" :src="imgLang[lang.id]">
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios'
import { cloneVNode } from '@vue/runtime-core'

export default {
  name: 'App',
  data(){
      return{
          devs:[],
          imgLang: {
              'JavaScript':'https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Unofficial_JavaScript_logo_2.svg/640px-Unofficial_JavaScript_logo_2.svg.png',
              'Python':'https://upload.wikimedia.org/wikipedia/commons/thumb/0/0a/Python.svg/800px-Python.svg.png',
              'Java':'https://inforchannel.com.br/wp-content/uploads/2021/03/e2d2f80e-java-logo.png',
              'Php':'https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/PHP-logo.svg/1200px-PHP-logo.svg.png',
          }
      }
  },
  created(){
      axios.get("https://bernardosantos.zeedhi.com/workfolder/dev.php").then((response) =>{
          this.devs=response.data.devs
          console.log(this.devs)
      })
  },
  methods:{
      abreModal: function(dev){
          let devLenght = dev.programmingLanguages.devLenght
          //var devLangs =''
          for(var i = 0; i < devLenght; i++){
              devLangs += '<div><div><img src="' +this.imgLang[dev.programmingLanguages[i]] + '"></div></div>'
            }
            console.log(this.devLangs)
            this.$modal.show(
            {
            template: `
            <div class="modal">
                <div>
                    <img src="${dev.picture}" style="height:128px; width:128px;">
                </div>
                <div>
                    <h4>${dev.name}</h4>
                    <span>${dev.age} anos</span>
                    <span>Contatos</span>
                    <span>${dev.email}</span>
                    <span>Linguagens</span>
                    <div>

                    </div>
                </div>
                <button style="border: none; background-color: white; height: 25px; font-size: 20px; " @click="$emit('close')">X</button>
            </div>
            `,
            props: ['text']
            },            
            { height: 'auto' },
            )
        }
    }
}


</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
  padding: 20px;
}

.listagem{
    display: grid;
    grid-template-columns: 33.3% 33.3% 33.3% ;
}

.info{
    padding-left: 20px;
}

img{
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px;
}

label{
    font-weight: bold;
}

.modal{
    display: grid;
    grid-template-columns: 25% 70% 5%;  
    padding: 20px;  
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
}

span{
    font-weight: bold;
    display: flex;
    padding-bottom: 10px;
}

.cabecalho{
    display: grid;
    grid-template-columns: 70% 30%;
}

.opcao{
    display: block;
}

.filtros{
    display: grid;
    grid-template-columns: 30% 70%;
}

.linguagens{
    display: inline-block
}

.card{
    box-shadow: 0 15px 15px 1px grey;
    border-radius: 10px;
    /*width: 500px;*/
    display: grid;
    margin: 10px 0 0 10px;
    grid-template-columns: 28% 72%;
    height: 128px;
}

.languages{
    width: 30px;
    height: 30px;
}

.name{
    font-weight: bold;
}
.age{
    font-weight: 400;
}


</style>
