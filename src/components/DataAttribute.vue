<template>
  <div class="container">
    <span class="item">{{ attr }}</span>
    <div v-if="hasChildren">
      <i
        v-on:click="toggleBtn($event)"
        data-toggle="false"
        class="fa fa-angle-down"
      ></i>
      <div class="children">
        <data-attribute
          v-for="(at, index) in value"
          v-bind:key="index"
          :attr="index"
          :value="at"
        ></data-attribute>
      </div>
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
    const childrenEl = event.target.nextSibling;
    const attr = event.target.attributes["data-toggle"];

    if (attr && attr.value === "true") {
      el.className = "fa fa-angle-down";
      childrenEl.classList.remove('hidden');
      attr.value = "false";
    } else {
      el.className = "fa fa-angle-up";
      childrenEl.classList.add('hidden');
      attr.value = "true";
    }


  }
}
</script>
<style scoped lang="less">
.item {
  color: black;
}
.children {
  padding-left: 1.5rem;
}
.container {
  max-width: 300px;
  text-align: start;
}
.hidden {
  display: none;
}
</style>