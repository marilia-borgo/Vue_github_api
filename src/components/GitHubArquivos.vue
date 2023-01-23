<template>
      <div class="listArquivos">
        <h1> Listar os arquivos do repositório</h1>
        <v-row>
          <v-col cols="12">
              <v-simple-table>
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-left">tipo</th>
                      <th class="text-left">link do arquivo</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="arquivo in arquivos" :key="arquivo.sha">
                      <td>{{ arquivo.type }}</td>
                      <td><a href="arquivo.html_url">{{ arquivo._links.html }}</a></td>
                    </tr>
                  </tbody>
                </template>
            </v-simple-table>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-progress-circular indeterminate color="primary" v-if="loading"></v-progress-circular>
            <v-btn color="primary" v-if="temmais" @click="listaConteudoRoot">MAIS</v-btn>
          </v-col>
        </v-row>
      </div>
    </template>
    
    <script>
    
      import {api} from '~api'
    
      export default {
        props: ['repo'],
        data: () => ({
          arquivos: [],
          loading: false,
          temmais: false,
          currentPage: 1
        }),
        methods: {
          async listaConteudoRoot(){
            this.loading = true
            const maisarquivos = await api.listaConteudoRoot(this.repo.owner.login, this.repo.name)
            this.arquivos = this.arquivos.concat(maisarquivos)
            console.log(this.arquivos)
            this.currentPage++
            this.loading = false
            this.temmais = maisarquivos.length > 0
          },
        },
        watch: {
          repo(){
            this.issues = []
            this.arquivos = []
            if (this.repo) {
              this.temmais = false
              this.currentPage = 1
              this.listaConteudoRoot()
            } else {
              this.arquivos=[]
              this.temmais = false
              this.currentPage = 1
            }
          }
        }
      }
    </script>
    