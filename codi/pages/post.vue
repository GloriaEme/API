<template>
    <v-container>
      <v-row>
        <v-col>
          <h1 style="font-size:10vh">{{ultimMissatge}}</h1>
          <h4>{{ultimUsuari}}</h4>  
        </v-col>
      </v-row>
    </v-container>
    </template>
    
    <script>
    export default{
      mounted(){
        console.log("abans de descarregar")
        this.descarregarUltimText()
        console.log("després de descarregar")
      },
      data(){
        return{
          respostaTest:null,
          ultimMissatge:"",
          ultimUsuari:""
        }
      },
      methods:{
        testejarApi(){
          console.log('Provant api..')
          let self = this
          this.$axios.get('https://last-one-smo-4w3qk.ondigitalocean.app/api')
            // Quan acabi
            .then(
              resposta=>{
                console.log("M'he descarregat les dades bé",resposta.data)
                self.respostaTest = resposta
                
              }
            )
            // Si hi ha errors
            .catch(
              error=>{
                console.log("M'he descarregat les dades malament",error)
              }
            )
        },
        descarregarUltimText(){
          let self = this
          this.$axios.get('https://last-one-smo-4w3qk.ondigitalocean.app/api/getText')
          .then(
            r=>{
              console.log('Ultim text',r)
              // Recollim
              let missatge = r.data.data.missatge
              let usuari = r.data.data.usuari
              // Assignem al data
              self.ultimMissatge = missatge
              self.ultimUsuari = usuari
            }
          )
          .catch(
            e=>{
              console.log('Ultim text error',e)
            }
          )
        }
      }
    }
    </script>