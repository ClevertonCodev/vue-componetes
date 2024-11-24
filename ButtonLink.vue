<template>
  <a :class="computedClasses" :href="href" @click="emitClickEvent">
    <i v-if="icon" :class="icon"></i>
    <span style="font-weight: bolder; padding: 10px">{{ text }}</span>
  </a>
</template>

<script>
export default {
  props: {
    customClass: {
      type: String,
    },
    text: {
      type: String,
    },
    icon: {
      type: String,
    },
    href: {
      type: String,
      default: null
    },
  },
  methods: {
    emitClickEvent() {
      this.$emit("click"); // Emitir evento "click" quando o link é clicado
    },
  },
  computed: {
    computedClasses() {
      return {
        "link-type": true,
        [this.customClass]: this.customClass,
      };
    },
  },
};
</script>

<style scoped>
.link-type {
  display: flex;
  align-items: baseline;
  justify-content: center;
  height: 42px;
  background: #ffffff;
  border: 2px solid #4f70e5;
  border-radius: 5px;
  opacity: 1;
  color: #4f70e5;
  font-size: 12px;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
}
.link-type:hover {
  color: #ffffff;
  background-color: #4f70e5;
}
@media (max-width: 500px) {
  .link-type {
    height: auto;
  }
}
</style>
