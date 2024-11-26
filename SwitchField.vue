<template>
<div class="toggle-container">
  <label
    class="toggle"
    :for="'field_' + name">
    <input
      :id="'field_' + name"
      v-model="input"
      type="checkbox"
      :name="name"
      :disabled="disabled"
      class="toggle__input">
    <span class="toggle-track">
      <span class="toggle-indicator">
        <span class="checkMark">
          <svg
            viewBox="0 0 24 24"
            role="presentation"
            aria-hidden="true">
            <path d="M9.86 18a1 1 0 01-.73-.32l-4.86-5.17a1.001 1.001 0 011.46-1.37l4.12 4.39 8.41-9.2a1 1 0 111.48 1.34l-9.14 10a1 1 0 01-.73.33h-.01z" />
          </svg>
        </span>
        <span class="uncheckMark">
          <i class="fas fa-times" />
        </span>
      </span>
    </span>
  </label>

  <span class="label-text">
    <info-field
      v-if="infoTitle"
      :info-title="infoTitle"
      :info-id-modal="'info-' + name"
      :large-modal="infoLarge"
      style="vertical-align: text-bottom;">
      <template #info>
        <slot name="info" />
      </template>
    </info-field>
    {{ label }}
  </span>
</div>
</template>

<script>
import InfoField from '../InfoField.vue';

export default {
    components: { InfoField, },
    props: {
        value: {
            type: Boolean,
            default: false,
            required: true,
        },
        name: {
            type: String,
            default: '',
            required: true,
        },
        label: {
            type: String,
            default: '',
            required: true,
        },
        disabled: {
            type: Boolean,
            default: false,
        },
        infoTitle: {
            type: String,
            default: '',
            required: false,
        },
        infoLarge: {
            type: Boolean,
            default: false,
        },
    },
    emits: ['input'],
    data() {
        return { input: Boolean(this.$props.value), };
    },
    watch: {
        value(newValue) {
            this.input = newValue;
        },
        input(input) {
            this.$emit('input', input);
        }
    },
};
</script>

<style scoped>
.toggle-container {
    min-height: 70px;
    display: flex;
    flex-direction: row;
    justify-content: start;
    align-items: center;
}

.toggle {
  align-items: center;
  border-radius: 100px;
  display: flex;
}
.label-text {
  color: #4A545A;
  font-size: 14px;
}

.toggle__input {
  clip: rect(0 0 0 0);
  clip-path: inset(50%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}


.toggle__input:disabled + .toggle-track {
  cursor: not-allowed;
  opacity: 0.7;
}

.toggle-track {
  background: #E6E0E9;
  border: 2px solid #79747E;
  border-radius: 100px;
  cursor: pointer;
  display: flex;
  height: 40px;
  margin-right: 12px;
  position: relative;
  min-width: 70px;
}

.toggle-indicator {
  align-items: center;
  background: #79747E;
  border-radius: 24px;
  bottom: 3px;
  display: flex;
  height: 30px;
  width: 30px;
  justify-content: center;
  left: 4px;
  outline: solid 2px transparent;
  position: absolute;
  transition: 0.25s;
}

.checkMark {
  fill: #6750A4;
  height: 20px;
  width: 20px;
  opacity: 0;
  transition: opacity 0.25s ease-in-out;
  display: none;
}

.uncheckMark {
  color: #E6E0E9;
  height: 20px;
  width: 20px;
  opacity: 0;
  transition: opacity 0.25s ease-in-out;
  display: none;
  text-align: center;
}

.toggle__input:checked + .toggle-track {
  background: #6750A4;
  border: 2px solid #6750A4;
}
.toggle__input:checked + .toggle-track .toggle-indicator {
  background: white;
  transform: translateX(30px);
}
.toggle__input:checked + .toggle-track .toggle-indicator .checkMark {
  display: inline;
  opacity: 1;
  transition: opacity 0.25s ease-in-out;
}
.toggle__input:not(:checked) + .toggle-track .toggle-indicator .uncheckMark {
  display: inline;
  opacity: 1;
  transition: opacity 0.25s ease-in-out;
}

@media screen and (-ms-high-contrast: active) {
  .toggle-track {
    border-radius: 0;
  }
}
</style>