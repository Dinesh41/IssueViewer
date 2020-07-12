<template>
  <div id="app">
    <div>
      <h1 style="text-align:center; font-size:50px; padding:10px;">Issue Viewer</h1>
      <div class="flx" style="justify-content:center;" v-if="errorFile == null && logFile == null">
        <label for="errorFile">Error Log file :</label>
        <input type="file" ref="errorFile" @change="errorFileUpload" id="errorFile" />
        <label for="logFile">User Log file :</label>
        <input type="file" ref="logFile" @change="logFileUpload" id="logFile" />
      </div>
      <div class="flx" style="justify-content:center;" v-if="errorFile">
        <h4 style="margin:0px 10px">
          Device Name :
          <span
            class="text-danger"
            style="font-size:30px"
            v-if="errorFile.device"
          >{{errorFile.device || logFile.device}}</span>
          <span class="text-danger" style="font-size:30px" v-else>-</span>
        </h4>
        <h4 style="margin:0px 10px">
          Model :
          <span
            class="text-danger"
            style="font-size:30px"
            v-if="errorFile.model"
          >{{errorFile.model}}</span>
          <span class="text-danger" style="font-size:30px" v-else>-</span>
        </h4>
        <h4 style="margin:0px 10px">
          App Version :
          <span
            class="text-danger"
            style="font-size:30px"
            v-if="errorFile.app_version"
          >{{errorFile.app_version}}</span>
          <span class="text-danger" style="font-size:30px" v-else>-</span>
        </h4>
      </div>
      <div class="flx" style="justify-content:center;" v-if="logFile">
        <h4 style="margin:0px 10px">
          Device Name :
          <span
            class="text-danger"
            style="font-size:30px"
            v-if="logFile.device"
          >{{logFile.device || logFile.device}}</span>
          <span class="text-danger" style="font-size:30px" v-else>-</span>
        </h4>
        <h4 style="margin:0px 10px">
          Model :
          <span
            class="text-danger"
            style="font-size:30px"
            v-if="logFile.model"
          >{{logFile.model}}</span>
          <span class="text-danger" style="font-size:30px" v-else>-</span>
        </h4>
        <h4 style="margin:0px 10px">
          App Version :
          <span
            class="text-danger"
            style="font-size:30px"
            v-if="logFile.app_version"
          >{{logFile.app_version}}</span>
          <span class="text-danger" style="font-size:30px" v-else>-</span>
        </h4>
      </div>
    </div>

    <ErrorViewer v-if="errorFile" :data="errorFile.data" :loading="errorFileLoading"></ErrorViewer>
    <LogViewer v-if="logFile" :data="logFile.data" :loading="logFileLoading"></LogViewer>
  </div>
</template>

<script>
import ErrorViewer from "./components/ErrorViewer.vue";
import LogViewer from "./components/LogViewer.vue";
export default {
  name: "App",
  data() {
    return {
      errorFile: null,
      logFile: null,
      errorFileLoading: false,
      logFileLoading: false
    };
  },
  components: {
    ErrorViewer,
    LogViewer
  },
  methods: {
    errorFileUpload() {
      let file = this.$refs.errorFile.files[0];
      if (!file || file.type !== "application/json") {
        alert("Please upload json file");
        return;
      }

      this.errorFileLoading = true;

      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload = evt => {
        console.log(evt.target.result);
        this.errorFile = JSON.parse(evt.target.result);
        this.errorFileLoading = false;
      };
      reader.onerror = evt => {
        alert("Error : " + evt);
      };
    },
    logFileUpload() {
      let file = this.$refs.logFile.files[0];
      if (!file || file.type !== "application/json") {
        alert("Please upload json file");
        return;
      }

      this.logFileLoading = true;

      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload = evt => {
        this.logFile = JSON.parse(evt.target.result);
        this.logFileLoading = false;
      };
      reader.onerror = evt => {
        alert("Error : " + evt);
      };
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
.flx {
  display: flex;
  flex-wrap: wrap;
}
.padd-0 {
  padding: 0 !important;
}
</style>
