<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");
const toast = useToast();

const emit = defineEmits(["transactionSubmited"]);

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("fields must have something");
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit("transactionSubmited", transactionData);

  text.value = "";
  amount.value = "";
};
</script>

<template>
  <h3>Add Transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" v-model="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (nagetive - expense, positive - income)
      </label>
      <input
        type="text"
        v-model="amount"
        id="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add Transaction</button>
  </form>
</template>
