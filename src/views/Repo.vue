<template>
  <div class="row d-flex justify-content-center">
    <b-card no-body class="overflow-auto" style="max-width: 1080px;">
      <b-row no-gutters>
        <b-col md="6">
          <b-card-img :src="avatarUrl" alt="Image" class="rounded-0"></b-card-img>
        </b-col>
        <b-col md="6">
          <b-card-body title="Name">
            <b-card-text>
              {{name}}
            </b-card-text>
          </b-card-body>
          <b-card-body title="No. of Watchers">
            <b-card-text>
              {{watchers}}
            </b-card-text>
          </b-card-body>
          <b-card-body title="No. of Forks">
            <b-card-text>
              {{forks}}
            </b-card-text>
          </b-card-body>
          <b-card-body title="No. of Open Issues">
            <b-card-text>
              {{openIssues}}
            </b-card-text>
          </b-card-body>
          <b-card-body title="License">
            <b-card-text>
              {{license}}
            </b-card-text>
          </b-card-body>
        </b-col>
      </b-row>
    </b-card>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    data() {
      return {
        avatarUrl: '',
        name: '',
        watchers: '',
        forks: '',
        openIssues: '',
        license: ''
      }
    },
    mounted() {
      const {organization,name} = this.$route.params;
      axios.get(`https://api.github.com/repos/${organization}/${name}`).then(({data})=>{
        this.name = data.name;
        this.avatarUrl = data.organization.avatar_url;
        this.watchers = data.watchers;
        this.forks = data.forks;
        this.openIssues = data.open_issues;
        this.license = data.license && data.license.name;
      });
    },
    methods: {
      countDownChanged(dismissCountDown) {
        this.dismissCountDown = dismissCountDown
      },
      showAlert() {
        this.dismissCountDown = this.dismissSecs
      }
    }
  }
</script>