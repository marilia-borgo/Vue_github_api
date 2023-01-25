<template>
<div><v-app id="inspire">
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-container class="py-0 fill-height">
        <v-btn
          v-for="link in links"
          :key="link"
          text
        >
          {{ link }}
        </v-btn>
        
        <v-spacer></v-spacer>
  
        </v-container>
      </v-app-bar>
      
      <v-main class="deep-purple lighten-4">
        <v-container>
          <v-row>
            <v-col cols="2">
              <v-sheet rounded="lg">
                <v-list color="transparent">
                  <p class="pa-3"> Futuramente aqui terá a tree de navegação entre pastas</p>
              
              <v-divider class="my-2"></v-divider>
              
                <v-list-item
                link
                  color="deep-purple lighten-5"
                  >
                </v-list-item>
              </v-list>
            </v-sheet>
          </v-col>
          
          <v-col>
            <v-sheet
            min-height="70vh"
            rounded="lg"
            class="pa-8"
            >
              <GithubRepo @reposelected="onRepoSelected"/>
              <GitHubArquivos :repo="repo"/>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
  <v-container>
  </v-container>
</div>

</template>

<script>
  import GithubRepo from './GithubRepo';
  import GitHubArquivos from './GitHubArquivos';
  export default {
    components: {
      GithubRepo,
      GitHubArquivos,
    },
    data: () => ({
      repo: null,
      links: [
        'Feito por:',
        'Marília',
        '2022'
      ],
    }),
    methods: {
      onRepoSelected(repo){
        this.repo = repo
      }
    }
  }
</script>
