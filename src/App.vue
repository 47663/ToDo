<script setup>
// importerar funktioner som behövs för projektet.
import { ref, onBeforeMount, watch } from 'vue';
//skapar object och array
const item = ref('');
const list = ref([]);

// tar in det man skrev in i textfältet och lägger till det till "list" efter att den har lagt till det man skrev in i våran array så skickar den det till localstorage 
function addItem() {
  let listItem = { text: item.value, done: false, class: '' };
  list.value.push(listItem);
  item.value = '';
  saveListToLocalStorage();
}
/* efter att man trycker på radera knappen på ett object i listan så kallar den på radera funktionen.
  efter detta så tar den indexet av den saken man vill ha bort och tar bort den ur vår array och sedan sparar detta till local storage.
*/
function radera(listItem) {
  list.value = list.value.filter(obj => {
    return obj !== listItem;
  });
  saveListToLocalStorage();
}
function toggleDone(itemToToggle) {
  itemToToggle.done = !itemToToggle.done;
  saveListToLocalStorage();
}
// skickar värdet av våran nuvarande lista till localstorage med namnet tasklist. 
function saveListToLocalStorage() {
  localStorage.setItem('taskList', JSON.stringify(list.value));
}

// denna funktion kallar på localstorage för våran lista som heter tasklist sedan så sätter den värdet av list till det som våran sparade lista har
function loadListFromLocalStorage() {
  const savedList = localStorage.getItem('taskList');
  if (savedList) {
    list.value = JSON.parse(savedList);
  }
}
// tar bort allting från localstorage
function removeLocalStorage() {
  if (localStorage.getItem('taskList')) {
    localStorage.removeItem('taskList');
    list.value = []; // Clear the list
  }
}
// innan hemsidan laddas så kallar den på listan som har blivigt sparad i localstorage
onBeforeMount(() => {
  loadListFromLocalStorage();
})
// letar efter förändringar i list och sparar allt i localstorage
watch(list, () => {
  saveListToLocalStorage();
})
</script>
<template>
  <header>
    <h1>Tasklist</h1>
  </header>

  <main>
    <div class="input-container">
      <input type="text" v-model="item" placeholder="Ange uppgift" class="input-field">
      <button @click="addItem" class="btn btn-primary">Lägg till</button>
    </div>
    <div class="button-container">
      <button @click="saveListToLocalStorage" class="btn btn-secondary full-width">Spara till Local Storage</button>
      <button @click="removeLocalStorage" class="btn btn-danger half-width">Ta bort lokal data</button>
    </div>
    <h3 id="todo">Todo...</h3>
    <ul>
      <template v-for="itm in list">
        <li v-if="!itm.done" v-bind:key="itm" class="todoItm" @click="itm.done = !itm.done">
          {{ itm.text }}
        </li>
      </template>
    </ul>
    <h3 id="klart"> Klart!</h3>
    <ul>
      <template v-for="itm in list">
        <li v-if="itm.done" v-bind:key="itm" class="todoItm done" @click="itm.done = !itm.done">
          {{ itm.text }}<span title="Radera" @click="radera(itm)">x</span>
        </li>
      </template>
    </ul>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

button {
  margin-left: 0.5em;
}

h1 {
  color: blue;
}

li span {
  display: inline-block;
  position: absolute;
  right: 3em;
  background-color: black;
  color: white;
  padding-left: 0.5em;
  padding-right: 0.5em;
  border: 2px solid transparent;
  border-radius: 4px;
  transition: border-color 0.3s;
}

li span:hover {
  border-color: #0078d4;
}

#todo {
  color: red;
}

.todoItm {
  margin-left: 2em;
  font-weight: bold;
  padding: 0.5em;
  list-style-type: none;
  cursor: pointer;
}

.todoItm:nth-child(odd) {
  background-color: darkred;
}

.todoItm:nth-child(even) {
  background-color: red;
}

.done:nth-child(odd) {
  background-color: green;
}

.done:nth-child(even) {
  background-color: darkgreen;
}

.done {
  text-decoration: line-through;
}

#klart {
  color: yellowgreen;
}

body {
  background-color: #f7f7f7;
  font-family: 'Arial', sans-serif;
}

h1 {
  color: #0078d4;
}

ul {
  list-style: none;
  padding: 0;
}

.button-container {
  display: flex;
  margin: 0 10px;
}

.input-field {
  flex: 1;
  padding: 10px;
  border: 2px solid #0078d4;
  border-radius: 4px;
  font-size: 16px;
}

.btn.btn-primary {
  background-color: #0078d4;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 10px 20px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn.btn-primary:hover {
  background-color: #005fbf;
}

.btn.btn-secondary {
  background-color: #ccc;
  color: #333;
  border: none;
  border-radius: 4px;
  padding: 10px 0;
  width: 100%;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn.btn-secondary:hover {
  background-color: #aaa;
}

.btn.btn-danger {
  background-color: #ff0000;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 10px 20px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn.btn-danger:hover {
  background-color: #cc0000;
}

.button-container {
  margin: 10px;
  text-align: center;
}</style>
