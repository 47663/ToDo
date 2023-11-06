<script setup>
import { ref, onBeforeMount } from 'vue';

const item = ref('');
const list = ref([]);

function addItem() {
  let listItem = { text: item.value, done: false, class: '' };
  list.value.push(listItem);
  item.value = '';
  saveListToLocalStorage();
}

function radera(listItem) {
  list.value = list.value.filter(obj => {
    return obj !== listItem;
  });
  saveListToLocalStorage();
}

function saveListToLocalStorage() {
  localStorage.setItem('taskList', JSON.stringify(list.value));
}

function loadListFromLocalStorage() {
  const savedList = localStorage.getItem('taskList');
  if (savedList) {
    list.value = JSON.parse(savedList);
  }
}

onBeforeMount(() => {
  loadListFromLocalStorage();
});
</script>

<template>
  <!-- Your existing template code remains the same -->
</template>
