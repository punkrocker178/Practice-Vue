<template>
  <div class="container">
    <span class="item">{{ attr }}</span>
    <span v-on:click="toggleBtn($event)" v-if="hasChildren"
      ><i data-toggle="false" class="fa fa-angle-down"></i
    ></span>
    <div v-if="hasChildren" class="children" :id="`children-${attr}`">
      <data-attribute
        v-for="(at, index) in value"
        v-bind:key="index"
        :attr="index"
        :value="at"
      ></data-attribute>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({ name: "data-attribute" })
export default class DataAttribute extends Vue {
  hasChildren: boolean;
  @Prop()
  attr!: string;
  @Prop()
  value!: any;

  data() {
    return {
      hasChildren: this.hasChildren
    };
  }

  created() {
    console.log(this.value);
    if (this.value && typeof this.value === "object") {
      this.hasChildren = true;
    }
  }

  toggleBtn(event) {
    const el = event.target;
    const attr = event.target.attributes["data-toggle"];

    if (attr && attr.value === "true") {
      el.className = "fa fa-angle-down";
      attr.value = "false";
    } else {
      el.className = "fa fa-angle-up";
      attr.value = "true";
    }
  }
}
</script>
<style scoped lang="less">
.item {
  color: black;
  display: inline-block;
  height: 30px;
  margin-bottom: 0.5rem;
}
.children {
  padding-left: 1.5rem;
}
.container {
  // display: flex;
  // flex-direction: row;
  max-width: 300px;
  text-align: start;
}
.hidden {
  display: none;
}
</style>