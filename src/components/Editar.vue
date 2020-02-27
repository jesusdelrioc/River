<template>
  <div class="modal-card" style="width: auto">
    <header class="modal-card-head">
      <p class="modal-card-title">Editar {{ todo.todo }}</p>
    </header>
    <section class="modal-card-body">
      <b-field label="Nombre">
        <b-input type="text" v-model="title" placeholder="Your todo title">
        </b-input>
      </b-field>

      <b-field label="Pais">
        <b-select placeholder="Selecciones el Pais" v-model="pais">
          <option v-for="option in priorities" :value="option.name" :key="option.id">
            {{ option.name }}
          </option>
        </b-select>
      </b-field>

      <b-field label="Longitud">
        <b-input type="text" v-model="longitud" placeholder="Longitud">
        </b-input>
      </b-field>

      <input type="file" @change="onFileChange" />

      <div id="preview">
        <img width=500px  v-if="url"  :src="url" />
      </div>

    </section>
    <footer class="modal-card-foot">
      <button class="button" type="button" @click="$parent.close()">
        Cerrar
      </button>
      <button class="button is-primary" @click="editTodo">Guardar</button>
    </footer>
  </div>
</template>

<script>
export default {
  name: "Editar",
  props: {
    todo: {
      type: Object,
      required: true
    },
    priorities: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      title: "",
      pais: "",
      longitud:'',
      url:''
    };
  },
  mounted() {
    this.title = this.todo.todo;
    this.pais = this.todo.pais;
    this.longitud = this.todo.longitud;
    this.url = this.todo.url;
  },
  methods: {
    editTodo() {
      const payload = {
        id: this.todo.id,
        todo: this.title,
        pais: this.pais,
        longitud: this.longitud,
        url: this.url
      };
      this.$emit("edit-todo", payload);
    },
     onFileChange(e) {
      const file = e.target.files[0];
      this.url = URL.createObjectURL(file);
    },
  }
};
</script>

<style scoped></style>
