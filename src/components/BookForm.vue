<template>
  <div>
    <div id="bookForm-container" class="flex flex-col items-center justify-center w-full">
      <h1 class="m-auto my-8 text-4xl shadow-2xl shadow-black w-120 text-shadow-md ">Gestiona el estado de tus libros</h1>

      <!-- Formulario para introducir libros en leído o pendiente usando v-if, v-else, v-for, v-model y v-bind -->
      <div id="form-container">
        <input v-model="bookCover" type="text" name="bookCover" id="bookCover" class="text-black p-2 m-2 w-full"
          placeholder="Introduce el enlace a la portada del libro">
        <div id="button-form-container">
          <button @click="saveBook('readedBooks', bookCover)"
            class="w-full border border-orange-300 hover:border-gray-500 cursor-pointer bg-gray-900 p-2 m-2">Guardar en
            leídos</button>
          <button @click="saveBook('pendantBooks', bookCover)"
            class="w-full border border-orange-300 hover:border-gray-500 cursor-pointer bg-gray-900 p-2 m-2">Guardar en
            pendientes</button>
        </div>
      </div>
      <!-- Fin formulario -->

      <!-- Details de libros leidos -->
      <details v-bind:open="readedBooks.length > 0" v-if="readedBooks.length > 0"
        class="flex flex-col items-center justify-center w-full">
        <summary class='mb-4 p-2 border border-orange-300 hover:border-gray-500 cursor-pointer w-2/4 m-auto bg-gray-900'>
          {{ `Has leido ${readedBooks.length} libro/s` }}
        </summary>
        <div class="flex flex-col justify-center items-center mb-4">
          <div v-for="(book, index) in readedBooks" :key="index" class="flex flex-col items-center mx-4">
            <img :src="book.cover" alt="Portada del libro" class="w-full md:w-50 lg:w-60 h-auto mb-2 md:mb-0">
            <button @click="removeBook('readedBooks', index)" class="text-red-500 cursor-pointer">Eliminar</button>
            <button @click="moveBook('readedBooks', index)" class="text-blue-500 cursor-pointer">Pendiente</button>
          </div>
        </div>
      </details>
      <details v-else class="flex flex-col items-center justify-center w-full">
        <summary class='mb-4 p-2 border border-orange-300 hover:border-gray-500 cursor-pointer w-2/4 m-auto bg-gray-900'>
          No has leido ningún libro
        </summary>
      </details>
      <!-- Fin details pendientes -->

      <!-- Details de libros pendientes -->
      <details v-bind:open="pendantBooks.length > 0" v-if="pendantBooks.length > 0"
        class="flex flex-col items-center justify-center w-full">
        <summary class='mb-4 p-2 border border-orange-300 hover:border-gray-500 cursor-pointer w-2/4 m-auto bg-gray-900'>
          {{ `Tienes ${pendantBooks.length} libros pendiente/s` }}
        </summary>
        <div class="flex flex-col justify-center items-center">
          <div v-for="(book, index) in pendantBooks" :key="index" class="flex flex-col items-center mx-4">
            <img :src="book.cover" alt="Portada del libro" class="w-full md:w-50 lg:w-60 h-auto mb-2 md:mb-0">
            <button @click="removeBook('pendantBooks', index)" class="text-red-500 cursor-pointer">Eliminar</button>
            <button @click="moveBook('pendantBooks', index)" class="text-blue-500 cursor-pointer">Leido</button>
          </div>
        </div>
      </details>
      <details v-else class="flex flex-col items-center justify-center w-full">
        <summary class='mb-4 p-2 border border-orange-300 hover:border-gray-500 cursor-pointer w-2/4 m-auto bg-gray-900'>
          No hay libros pendientes
        </summary>
      </details>
      <!-- Fin details pendientes -->
    </div>
  </div>
</template>
  
<script setup>
import { ref } from 'vue';

const bookCover = ref('');

const saveBook = (list, coverUrl) => {
  const newBook = { cover: coverUrl };
  // Agrega el nuevo libro a la lista correspondiente
  if (list === 'readedBooks') {
    readedBooks.value.push(newBook);
  } else if (list === 'pendantBooks') {
    pendantBooks.value.push(newBook);
  }
  // Limpia el input después de guardar el libro
  bookCover.value = '';
};

const removeBook = (list, index) => {
  if (list === 'readedBooks') {
    readedBooks.value.splice(index, 1);
  } else if (list === 'pendantBooks') {
    pendantBooks.value.splice(index, 1);
  }
};

const moveBook = (fromList, index) => {
  const bookToMove = fromList === 'readedBooks' ? readedBooks.value[index] : pendantBooks.value[index];
  const toList = fromList === 'readedBooks' ? 'pendantBooks' : 'readedBooks';

  // Almacena la URL de la portada antes de eliminar el libro de la lista original
  const coverUrl = bookToMove.cover;
  console.log(coverUrl);

  removeBook(fromList, index);

  // Usa la URL almacenada al agregar el libro a la nueva lista
  saveBook(toList, coverUrl);
};

const readedBooks = ref([]);
const pendantBooks = ref([]);
</script>
<style scoped>
.text-shadow-md {
  text-shadow: 10px 10px 10px #b67600, -10px -10px 10px #b67600, 10px -10px 10px #b67600, -10px 10px 10px #b67600;
}
</style>