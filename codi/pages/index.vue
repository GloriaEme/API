<template>
  <v-container>
    <v-row>
      <v-col>
        <v-text-field v-model="missatge" label="Missatge"></v-text-field>
      </v-col>
      <v-col>
        <v-text-field v-model="usuari" label="Usuari"></v-text-field>
      </v-col>
      <v-col>
        <v-btn @click="enviarMissatge()">enviar</v-btn>
      </v-col>
    </v-row>
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
        this.descarregarUltimText()
    },
    data(){
      return{
        respostaTest:null,
        ultimMissatge:"",
        ultimUsuari:"",
        missatge:"",
        usuari:""
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
            //console.log('Ultim text',r)
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
      },
      enviarMissatge(){
        let self = this
        let dades = {
          missatge:this.missatge,
          usuari:this.usuari
        }
        let url = 'https://last-one-smo-4w3qk.ondigitalocean.app/api/setText'
        this.$axios.post(url,dades)
          .then(
            r=>{
              console.log("S'ha enviat bé",r)
              if(r.data.data.resultat == "fail"){
                alert(r.data.data.error)
              }else{
                self.missatge = ""
                self.descarregarUltimText()
              }
            }
          )
          .catch(
            e=>{
              console.log("No s'ha enviat bé",e)
            }
          )
      }
    }
  }
  </script>