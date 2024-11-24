<template>
  <label
    :class="{
      switch: true,
      disabled: disabledLocation,
    }"
  >
    <input type="checkbox" v-model="input" :disabled="disabledLocation"/>
    <span :class="{ slider: !isSlider, sliderActive: isSlider }"></span>
    <input
      type="hidden"
      :name="$props.name"
      :value="
        input
          ? $props.yesValue != undefined
            ? $props.yesValue
            : 1
          : $props.noValue != undefined
          ? $props.noValue
          : 0
      "
    />
  </label>
</template>
<script>
import axios from "axios";
export default {
  props: [
    "value",
    "name",
    "label",
    "yesLabel",
    "noLabel",
    "yesValue",
    "noValue",
    "disabled",
    "isActive",
    "methodPay",
    "csrf",
  ],
  data() {
    return {
      input: Boolean(this.$props.value),
      isSlider: Boolean(this.$props.isActive),
      disabledLocation: Boolean(this.$props.disabled),
    };
  },
  watch: {
    input(newValue) {
      this.UpdadeStatusMethods(newValue);
    },
    disabled(newValue) {
      this.disabledLocation = Boolean(newValue);
      if (newValue) {
        this.input = false;
      }
    },
  },
  methods: {
    async UpdadeStatusMethods(active) {
      let data = new FormData();
      data.append("ativo", active);
      data.append("metodo", this.$props.methodPay);
      data.append("csrf", this.csrf);
      try {
        this.disabledLocation = true;
        const response = await axios.post(
          `/admin/configurar-pagamentos/updade/status`,
          data
        );
        if (response.data.success) {
          this.$emit('update-methods');
        }
        this.disabledLocation = false;
      } catch (error) {
        this.input = false;
        if (error.response && error.response.data && error.response.data.erro) {
          this.$emit('on-error', error.response.data.erro)
        } else {
          this.$emit('on-error',"Ocorreu um erro desconhecido, tente novamente!");
        }
      }
    },
  },
};
</script>
<style scoped>
input:checked + .sliderActive:after {
  position: absolute;
  content: "Ativo";
  left: 50px;
  font-family: Poppins;
  font-size: 12px;
  font-weight: 600;
  line-height: 10px;
  letter-spacing: 0em;
  color:#1fd295;
  bottom: 0px;
}

.sliderActive:after {
  position: absolute;
  content: "Inativo";
  left: 50px;
  color: #DC4747;
  font-family: Poppins;
  font-size: 12px;
  font-weight: 600;
  line-height: 10px;
  letter-spacing: 0em;
  bottom: 0px;
}
.slider-disable {
  opacity: 0.4;
}
</style>
