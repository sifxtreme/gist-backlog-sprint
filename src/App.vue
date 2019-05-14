<template>
  <div id="app">
    <div class="container">
      <h2>Backlog</h2>
      <div v-html="backlog"></div>
    </div>
    <div class="container">
      <h2>Current Sprint</h2>
      <div v-html="currentSprint"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import showdown from "showdown";

export default {
  name: "app",
  components: {},
  data() {
    return {
      backlog: null,
      currentSprint: null
    };
  },
  async created() {
    const gist = await axios.get(
      `https://api.github.com/gists/${process.env.VUE_APP_GIST_ID}`,
      {
        headers: {
          Authorization: `token ${process.env.VUE_APP_GIST_GITHUB_TOKEN}`
        }
      }
    );

    const converter = new showdown.Converter();
    converter.setFlavor("github");

    this.backlog = converter.makeHtml(gist.data.files["backlog.md"].content);
    this.currentSprint = converter.makeHtml(
      gist.data.files["current.md"].content
    );
  }
};
</script>

<style>
html,
body {
  height: 100%;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: white;
  margin-top: 10px;
  background-color: #424242;
}
a {
  color: #88e188;
}

#app {
  display: flex;
}

.container {
  flex: 50%;
}
</style>
