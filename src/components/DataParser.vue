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
    <data-attribute
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
  url: string;
  
  data() {
    return {
      attributes: this.attributes,
      url: this.url
    };
  }

  callApi(event) {
    const res = this.getData(this.url);
    res.then(data => {
      this.attributes = data;
    }).catch(e => {
      console.error(e);
    });
  }

  async getData(url: string) {
    const res = await fetch(
      url,
      {
        headers: {
          "Content-Type": "application/json"
        },
        method: "GET"
      }
    );
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
</style>
