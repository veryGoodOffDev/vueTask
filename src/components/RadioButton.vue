<template>
  <div class="buttons__container">
    <label v-for="(option, index) in options" :key="index" @click="changeClass">
      <input
        type="radio"
        :value="option"
        v-model="selectedOpt"
        name="radioGroup"
      />{{ option }}
    </label>
  </div>
</template>
<script setup>
import { defineProps, watch, ref, defineEmits, reactive } from "vue";

defineEmits(["update:selectedOption"]);
const props = defineProps({
  options: Array,
  selectedOption: String,
});


const selectedOpt = ref(null);

watch(
  () => props.selectedOption,
  (newValue, oldValue) => (selectedOpt.value = newValue)
);
</script>

<style scoped>
.buttons__container {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.buttons__container input {
  appearance: none;
}

.buttons__container label {
  text-align: center;
  padding: 10px 80px;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 5px;
  -webkit-box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
  -moz-box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
  box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
}
.checked {
  -webkit-box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2) inset;
  -moz-box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2) inset;
  box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2) inset;
}

@media (max-width:580px) {
  .buttons__container {
    flex-direction: column;
    gap: 20px;
  }
}

</style>
