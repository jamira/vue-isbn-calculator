<script setup>
import { ref, computed, watch } from 'vue';

const ISBN = ref('');
const completedISBN = ref('');

const isValidISBN = computed(() => {
  return ISBN.value.length === 12 && /^\d+$/.test(ISBN.value);
});

const formattedISBN = computed(() => {
  const IBSN = completedISBN.value.split('');
  return IBSN.map((digit, index) => {
    if (index === 0) return digit;
    if (index === 3 || index === 8 || index === 12) return '-' + digit;
    return digit;
  }).join('');
});

watch(ISBN, (newISBN) => {
  const getDigits = newISBN.slice(0, 12);
  const digits = getDigits.split('').map((digit) => Number(digit));

  const sum = digits.reduce((acc, digit, index) => {
    return acc + (index % 2 === 0 ? digit : digit * 3);
  }, 0);

  const checkDigit = (10 - (sum % 10)) % 10;

  completedISBN.value = newISBN + checkDigit.toString();
});
</script>

<template>
  <div class="field_input_wrapper field__input">
    <label for="isbn">ISBN</label>
    <input
      type="text"
      id="isbn"
      name="isbn"
      class="field__input_input"
      v-model.trim="ISBN"
    />
    <p v-if="isValidISBN">Complete ISBN: {{ completedISBN }}</p>
    <p v-else>Invalid ISBN</p>
  </div>
</template>

<style scoped>
.field_input_wrapper {
  display: flex;
  flex-direction: column;
  margin-bottom: 1em;
  text-align: left;
}

.field__input_input {
  box-sizing: border-box;
  padding: 0.5em;
  border-radius: 5px;
  outline: none;
  border-radius: 8px;
  border: 1px solid #d6d9e6;
  height: 35px;
}
</style>
