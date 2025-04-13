<template>
  <v-card flat>
    <v-card-title>Students</v-card-title>

    <v-btn color="primary" class="mb-4" @click="openDialog">Add student</v-btn>

    <v-text-field
      v-model="search"
      label="Search"
      prepend-inner-icon="mdi-magnify"
      variant="outlined"
      hide-details
      single-line
      class="mt-4"
    ></v-text-field>

    <v-data-table
      :headers="headers"
      :items="students"
      :search="search"
      item-value="id"
      class="mt-4"
    >
      <template v-slot:[`item.actions`]="{ item }">
        <v-btn icon color="blue" @click="startEditing(item)">
          <v-icon>mdi-pencil</v-icon>
        </v-btn>
        <v-btn icon color="red" @click="deleteStudent(item.id)">
          <v-icon>mdi-delete</v-icon>
        </v-btn>
      </template>
    </v-data-table>

    <v-dialog v-model="dialog" max-width="600px">
      <v-card>
        <v-card-title>
          <span class="text-h6">{{ editingStudent ? 'Edit student' : 'Add student' }}</span>
        </v-card-title>
        <v-card-text>
          <v-form>
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field v-model="newStudent.name" label="ПІП" required></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="newStudent.birthYear"
                  label="Рік народження"
                  type="date"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="newStudent.phone"
                  label="Телефон"
                  type="phone"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="newStudent.email"
                  label="Email"
                  type="email"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="closeDialog">Скасувати</v-btn>
          <v-btn color="primary" @click="saveStudent">Зберегти</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script setup>
import { ref } from 'vue'

const dialog = ref(false)
const search = ref('')
const newStudent = ref({
  name: '',
  birthYear: '',
  phone: '',
  email: '',
})
const editingStudent = ref(null)
const students = ref([
  {
    id: 1,
    name: 'Іван Іванов',
    birthYear: 2000,
    phone: '123-456-789',
    email: 'ivanov@example.com',
  },
  {
    id: 2,
    name: 'Петро Петренко',
    birthYear: 1999,
    phone: '987-654-321',
    email: 'petrenko@example.com',
  },
])
const headers = [
  { key: 'name', title: 'ПІП' },
  { key: 'birthYear', title: 'Рік народження' },
  { key: 'phone', title: 'Телефон' },
  { key: 'email', title: 'Email' },
  { key: 'actions', title: 'Дії', sortable: false },
]

const openDialog = () => {
  newStudent.value = { name: '', birthYear: '', phone: '', email: '' }
  editingStudent.value = null
  dialog.value = true
}

const closeDialog = () => {
  dialog.value = false
}

const saveStudent = () => {
  if (
    newStudent.value.name &&
    newStudent.value.birthYear &&
    newStudent.value.phone &&
    newStudent.value.email
  ) {
    if (editingStudent.value) {
      const index = students.value.findIndex((s) => s.id === editingStudent.value.id)
      if (index !== -1) {
        students.value[index] = { id: editingStudent.value.id, ...newStudent.value }
      }
    } else {
      students.value.push({
        id: Date.now(),
        ...newStudent.value,
      })
    }
    closeDialog()
  }
}

const startEditing = (student) => {
  editingStudent.value = { ...student }
  newStudent.value = { ...student }
  dialog.value = true
}

const deleteStudent = (id) => {
  students.value = students.value.filter((student) => student.id !== id)
}
</script>
