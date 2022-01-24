<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <template v-for="branch in branches">
      <input
        type="radio"
        :id="branch"
        :key="branch"
        :value="branch"
        name="branch"
        v-model="currentBranch"
      />
      <label :key="branch.sha" for="branch">{{ branch }}</label>
    </template>
    <p>IBM/AMLSim@{{ currentBranch }}</p>
    <ul>
      <li v-for="commit in commits"
      :key="commit.sha">
        <a :href="commit.html_url">{{ commit.sha | trucate }}</a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    title: String,
  },
  created: function () {
    this.fetchData();
  },
  data: function () {
    const branches = ["master", "new-schema"];
    return {
      branches: branches,
      commits: [],
      currentBranch: branches[0],
    };
  },
  watch: {
    currentBranch: "fetchData",
  },
  filters: {
    trucate: function(v) {
      return v.slice(0, 7)
    }
  },
  methods: {
    fetchData: function () {
      fetch(this.apiUrl())
        .then((response) => response.json())
        .then((data) => {
          this.commits = data;
        });
    },
    apiUrl: function () {
      return (
        "https://api.github.com/repos/ibm/amlsim/commits?per_page=3&sha=" +
        this.currentBranch
      );
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
