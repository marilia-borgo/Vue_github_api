<template>
  <v-container>
    <GithubRepo @reposelected="onRepoSelected"/>
    <GithubIssues :repo="repo"/>
    <GitHubArquivos :repo="repo"/>
  </v-container>
</template>
  
<script>
  import GithubRepo from './GithubRepo';
  import GithubIssues from './GithubIssues';
  import GitHubArquivos from './GitHubArquivos';
  export default {
    components: {
      GithubRepo,
      GithubIssues,
      GitHubArquivos,
    },
    data: () => ({
      repo: null
    }),
    methods: {
      onRepoSelected(repo){
        this.repo = repo
      }
    }
  }
</script>
