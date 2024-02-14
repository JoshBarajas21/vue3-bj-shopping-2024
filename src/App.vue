<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { ref } from "vue";

const header = ref("🛒 Shopping List App");
const items = ref([
  { id: 1, label: "10 bolillos", purchased: true, highPriority: true },
  { id: 2, label: "1 lata de frijoles", purchased: false, highPriority: true },
  { id: 3, label: "2 latas de atún", purchased: true, highPriority: false },
]);
const newItem = ref("");
const newItemHighPriority = ref(false);
// Método para agregar nuevos elementos a la lista
const saveItem = () => {
  items.value.push({ 
    id: items.value.length + 1, 
    label: newItem.value,
    highPriority: newItemHighPriority.value
  });
  newItem.value = "";
};
// Boton que oculta el formulario
const editing = ref(false);
// Funcion que alterna el valor de la variable editing
const doEdit = (edit) => {
  editing.value = edit;
  // Limpiando la entrada de texto
  // en caso de que se oculte o muestre
  // el formulario
  newItem.value = "";
};
// Alternando estado de compra del item
const togglePurchased = (item) => {
  item.purchased = !item.purchased;
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <!-- Botones de interfaz del usuario -->
    <button v-if="editing" @click="doEdit(false)" class="btn btn-cancel">Cancel</button>
    <button v-else @click="doEdit(true)" class="btn btn-primary">Add Item</button>
  </div>
  <!-- Hiper enlace enlazado -->
  <!-- <a v-bind:href="newItem">Dinamic Link</a> -->
   
  <!-- Agrupando en un div las entradas -->
  <form
    v-if="editing"
    v-on:submit.prevent="saveItem"
    class="add-item-form"
  >
    <!-- entrada de texto -->
    <input
      v-model.trim="newItem"
      type="text"
      placeholder="Add Item"
    />
    <!-- Caja de seleccion de prioridad -->
    <label>
      <input type="checkbox"
      v-model="newItemHighPriority"/>
      High Priority
    </label>
    <!-- Boton -->
    <button
    :disabled="newItem.length === 0"
      class="btn btn-primary"
      type="submit"
    >Save Item</button>
  </form>
  <!-- Lista -->
  <ul>
    <li v-for="item in items"
      @click="togglePurchased(item)"
      v-bind:key="item.id"
      :class="{strikeout: item.purchased, priority: item.highPriority }"
      > ⚜ {{ item.label }}</li>
  </ul>
  <p v-if="items.length === 0"> 🥀 No hay elementos en la lista</p>
</template>