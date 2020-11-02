<template>
  <div class="container">
    <h1>Tree viewer</h1>
    <div>
      <h2>API url:</h2>
      <input
        class="input"
        type="text"
        placeholder="https://"
        v-model="url"
        v-on:keyup.enter="callApi"
      />
    </div>

    <div v-show="isLoading">
      <i class="fa fa-spinner custom-spinner" aria-hidden="true"></i>
    </div>

    <data-attribute
      v-show="!isLoading"
      v-for="(attr, index) of attributes"
      v-bind:key="`${index}`"
      v-bind="attributes"
      :attr="index"
      :value="attr"
    ></data-attribute>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import DataAttribute from "./DataAttribute.vue";

@Component({
  components: {
    DataAttribute
  }
})
export default class DataParser extends Vue {
  attributes: any;
  isLoading: boolean;
  url: string;

  data() {
    return {
      attributes: this.attributes,
      isLoading: this.isLoading,
      url: this.url
    };
  }

  callApi(event) {
    this.isLoading = true;
    const res = this.getData(this.url);
    res
      .then(data => {
        this.isLoading = false;
        this.attributes = data;
      })
      .catch(e => {
        console.error(e);
      });
  }

  async getData(url: string) {
    const res = await fetch(url, {
      headers: {
        "Content-Type": "application/json"
      },
      method: "GET"
    });
    return res.json();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.container {
  width: 60%;
  display: flex;
  flex-direction: column;
  margin: 0 auto;
}
.input {
  width: 50%;
  border-radius: 4px;
  margin-bottom: 1rem;
  padding: 0.5rem;
}

.custom-spinner {
  font-size: 40px;
}

.fa-spinner::before {
  animation: spinner 1s linear infinite;
  display: inline-block;
}

@keyframes spinner {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>
