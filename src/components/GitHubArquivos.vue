<template>
      <div class="listArquivos">
        <v-banner two-line>
    <v-avatar
      slot="icon"
      color="deep-purple lighten-2"
      size="40"
    >
      <v-icon
        color="white"
      >
      mdi-file-multiple
      </v-icon>
    </v-avatar>
    Caminho:
    <div v-if="currentPath">{{ currentPath }}</div>
    <div v-else>root</div>
    <template v-slot:actions>
    </template>
  </v-banner>
        <v-row>
          <v-col cols="12">
            <template>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Repository Arquivos</th>
                  <th class="text-left">Link to arquivo</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="content in arquivos" :key="content.number">
                  <td class="text-left" v-if="content.type == 'dir'">
                    <v-btn
                      x-small
                      color="deep-purple lighten-2"
                      @click="listaConteudoInside(content.path)"
                    >
                      {{ content.name }}
                    </v-btn>
                  </td>
                  <td v-else class="text-left">{{ content.name }}</td>
                  <td class="text-left">{{ content.html_url}}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </template>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">

            <v-progress-circular indeterminate color="deep-purple lighten-2" v-if="loading"></v-progress-circular>
            <v-btn v-if="arquivosInside.length > 0" class="mb-5" @click="voltarPasta()">
            Voltar
          </v-btn>
            <v-btn color="primary" v-if="temmais" @click="listaConteudoRoot()">MAIS</v-btn>

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
          arquivosInside: [],
          currentPath: null,
          loading: false,
          temmais: false,
          currentPage: 1
        }),
        methods: {
          async listaConteudoRoot(){
            this.loading = true
            const maisarquivos = await api.listaConteudoRoot(this.repo.owner.login, this.repo.name)
            this.arquivos = this.arquivos.concat(maisarquivos)
            this.currentPage++
            this.loading = false
            this.temmais = maisarquivos.length > 0
          },
          async listaConteudoInside(path){
            this.loading = true
            this.arquivos = await api.listaConteudoInside(this.repo.owner.login,
                this.repo.name,
                path)
              this.arquivosInside.push(path);
              this.currentPath = path;
              this.loading = false;
          },
            async voltarPasta() {
              this.loading = true;
              this.arquivosInside.pop();
              let pastaAnterior =
                this.arquivosInside.length == 1 ? this.arquivosInside[0] : this.arquivosInside[-1];
              if (pastaAnterior == undefined) {
                pastaAnterior = "";
              }
              this.arquivos = await api.listaConteudoInside(
                this.repo.owner.login,
                this.repo.name,
                pastaAnterior
              );
              this.currentPath = pastaAnterior;
              this.loading = false;
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
    