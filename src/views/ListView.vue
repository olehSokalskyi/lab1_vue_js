<script setup lang="ts"></script>

<template>
  <main>
    <v-text-field
      v-model="newItemTitle"
      label="Add new item"
      append-icon="mdi-plus"
      clearable
      @click:append="addNewItem"
      @keyup.enter="addNewItem"
    ></v-text-field>

    <v-list v-model:selected="shoppingSection" select-strategy="leaf">
      <v-subheader>Settings</v-subheader>
      <div v-for="(item, index) in shoppingList" :key="item.id">
        <v-list-item
          :title="item.title"
          @click="doneBought(item.id)"
          :class="{ 'blue lighten-5': item.bought }"
        >
          <template v-slot:prepend>
            <v-list-item-action start>
              <v-checkbox-btn :model-value="item.bought"></v-checkbox-btn>
            </v-list-item-action>
          </template>
          <template v-slot:title>
            <v-list-item-title :class="{ 'text-decoration-line-through': item.bought }">
              {{ item.title }}
            </v-list-item-title>
          </template>
          <template v-slot:append>
            <v-list-item-action>
              <v-btn icon @click.stop="deleteItem(item.id)">
                <i class="mdi mdi-delete"></i>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider v-if="index < shoppingList.length - 1"></v-divider>
      </div>
    </v-list>
  </main>
</template>

<script setup>
import { ref } from 'vue'

const shoppingList = ref([
  {
    id: 1,
    title: 'Groceries',
    bought: false,
  },
  {
    id: 2,
    title: 'Electronics',
    bought: false,
  },
  {
    id: 3,
    title: 'Clothing',
    bought: false,
  },
])

const newItemTitle = ref('')

function addNewItem() {
  if (newItemTitle.value.trim() !== '') {
    shoppingList.value.push({
      id: Date.now(),
      title: newItemTitle.value.trim(),
      bought: false,
    })
    newItemTitle.value = '' 
  }
}

function doneBought(id) {
  const item = shoppingList.value.find((item) => item.id === id)
  if (item) {
    item.bought = !item.bought 
  }
}

function deleteItem(id) {
  shoppingList.value = shoppingList.value.filter((item) => item.id !== id)
}

const shoppingSection = ref([])
</script>