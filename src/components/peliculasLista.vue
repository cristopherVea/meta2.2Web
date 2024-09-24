<!-- src/components/Peliculas.vue -->
<template>
  <v-data-table
    :headers="headers"
    :items="peliculas"
    :sort-by="[{ key: 'rating', order: 'desc' }]"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Películas CRUD</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ props }">
            <v-btn class="mb-2" color="primary" dark v-bind="props">
              Nueva Película
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.movie"
                      label="Título"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.rating"
                      label="Calificación"
                      type="number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.imdb_url"
                      label="URL IMDB"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="close">
                Cancelar
              </v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="save">
                Guardar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >¿Está seguro de que desea eliminar esta película?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete"
                >Cancelar</v-btn
              >
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon size="small" class="me-2" @click="editItem(item)">
        mdi-pencil
      </v-icon>
      <v-icon size="small" @click="deleteItem(item)"> mdi-delete </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize"> Reiniciar </v-btn>
    </template>
  </v-data-table>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

const peliculas = ref([]);
const dialog = ref(false);
const dialogDelete = ref(false);

const headers = [
  { title: "Título", align: "start", key: "movie" },
  { title: "Calificación", key: "rating" },
  { title: "URL IMDB", key: "imdb_url" },
  { title: "Acciones", key: "actions", sortable: false },
];

const editedIndex = ref(-1);
const editedItem = ref({
  id: "",
  movie: "",
  rating: 0,
  imdb_url: "",
});
const defaultItem = {
  id: "",
  movie: "",
  rating: 0,
  imdb_url: "",
};

const formTitle = computed(() => {
  return editedIndex.value === -1 ? "Nueva Película" : "Editar Película";
});

onMounted(async () => {
  await initialize();
});

async function initialize() {
  try {
    const response = await fetch("https://dummyapi.online/api/movies");
    const data = await response.json();
    peliculas.value = data;
  } catch (error) {
    console.error("Error al obtener las películas:", error);
  }
}

function editItem(item) {
  editedIndex.value = peliculas.value.indexOf(item);
  editedItem.value = Object.assign({}, item);
  dialog.value = true;
}

function deleteItem(item) {
  editedIndex.value = peliculas.value.indexOf(item);
  editedItem.value = Object.assign({}, item);
  dialogDelete.value = true;
}

function deleteItemConfirm() {
  peliculas.value.splice(editedIndex.value, 1);
  closeDelete();
}

function close() {
  dialog.value = false;
  editedIndex.value = -1;
  editedItem.value = Object.assign({}, defaultItem);
}

function closeDelete() {
  dialogDelete.value = false;
  editedIndex.value = -1;
  editedItem.value = Object.assign({}, defaultItem);
}

function save() {
  if (editedIndex.value > -1) {
    Object.assign(peliculas.value[editedIndex.value], editedItem.value);
  } else {
    peliculas.value.push(editedItem.value);
  }
  close();
}
</script>
