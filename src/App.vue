<template>
  <div class="max-w-3xl mx-auto p-6 font-sans text-gray-500">
    <h2 class="text-2xl font-bold mb-4 text-gray-900 text-center">Registrar Contacto</h2>
    
    <form @submit.prevent="guardar" class="space-y-4 max-w-sm mx-auto">
      <input 
        v-model="form.nombre" 
        placeholder="Nombre" 
        required 
        class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
      />

      <input 
        v-model="form.email" 
        placeholder="Correo" 
        required 
        class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
      />

      <input 
        v-model="form.telefono" 
        placeholder="Teléfono" 
        required 
        class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
      />

      <button 
        type="submit" 
        class="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-md shadow-sm transition-colors duration-150 cursor-pointer"
      >
        Guardar
      </button>
    </form>

    <hr class="my-8 border-gray-200" />

    <h2 class="text-2xl font-bold mb-4 text-gray-900 text-center">Lista de Contactos</h2>
    
    <div class="overflow-x-auto">
      <table class="w-full border-collapse text-left border border-gray-200 shadow-sm rounded-lg overflow-hidden">
        <thead class="bg-gray-50 border-b border-gray-200">
          <tr>
            <th class="p-3 text-sm font-semibold text-gray-600">Nombre</th>
            <th class="p-3 text-sm font-semibold text-gray-600">Correo</th>
            <th class="p-3 text-sm font-semibold text-gray-600">Teléfono</th>
            <th class="p-3 text-sm font-semibold text-gray-600">Estatus</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200 bg-white">
          <tr v-for="c in contactos" :key="c.id" class="hover:bg-gray-50 transition-colors">
            <td class="p-3 text-sm text-gray-700">{{ c.nombre }}</td>
            <td class="p-3 text-sm text-gray-700">{{ c.email }}</td>
            <td class="p-3 text-sm text-gray-700">{{ c.telefono }}</td>
            <td class="p-3 text-sm text-gray-700">
              <span 
                :class="c.status ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'" 
                class="px-2 py-1 rounded-full text-xs font-semibold"
              >
                {{ c.status ? 'Activo' : 'Inactivo' }}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const contactos = ref([])
const form = ref({ nombre: '', email: '', telefono: '' })

const cargarContactos = async () => {
  const res = await fetch('http://127.0.0.1:8000/api/contactos')
  contactos.value = await res.json()
}

const guardar = async () => {
  await fetch('http://127.0.0.1:8000/api/contactos', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(form.value)
  })
  form.value = { nombre: '', email: '', telefono: '' }
  cargarContactos()
}

onMounted(() => {
  cargarContactos()
})
</script>