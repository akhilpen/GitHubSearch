<template>
  <div class="row d-flex justify-content-center">
    <div class="w-75">
      <b-alert v-if="error" show variant="danger">{{error}}</b-alert>
      <Search  v-model="searchInput" />
      <br>
      <Table class="table-bordered" :items="repos"  @rowClick="rowClick"/>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
// @ is an alias to /src
import Search from "@/components/Search.vue";
import Table from "@/components/Table.vue";

export default {
  name: "Home",
  components: {
    Search,
    Table,
  },
  data() {
    return {
      searchInput: '',
      repos: [],
      error: false
    };
  },
  methods: {
    rowClick(item){
      const {organization,name} = item;
      this.$router.push(`/${organization}/${name}`);
    },
    getData(key){
      this.repos = [];
      this.error = null;
      axios.get(`https://api.github.com/orgs/${key}/repos`)
        .then(({data})=>this.repos = data.map(i => {
          return {
            organization: i.owner && i.owner.login,
            name: i.name
          }
        }) || [])
        .catch(()=>{
          this.error = `Something went wrong! Please try again with a different Organization.`
        });
    }
  },
  mounted() {
    const {organization} = this.$route.params;
    if(organization) {
      this.getData(organization);
      this.searchInput = organization;
    }
  },
  watch: {
    searchInput(input) {
       const {organization} = this.$route.params;
       if(organization != input)
        this.$router.push(`/${input}`);
    },
    $route() {
      const {organization} = this.$route.params;
      if(organization) {
        this.getData(organization);
        this.searchInput = organization;
      }
    }
  },
};
</script>
