<template>
  <div class="modal-card" style="width: auto">
    <header class="modal-card-head">
      <p class="modal-card-title">Añadir</p>
    </header>
    <section class="modal-card-body">
      <b-field label="Nombre">
        <b-input type="text" v-model="title" placeholder="Nombre del rio" required>
        </b-input>
      </b-field>

      <b-field label="Pais">
        <b-select placeholder="Selecciones el Pais"  v-model="pais" required>
          <option v-for="option in priorities" :value="option.name" :key="option.id">
            {{ option.name }}
          </option>
        </b-select>
      </b-field>

      <b-field label="Longitud">
        <b-input type="number" v-model="longitud" placeholder="Km" required>
        </b-input>
      </b-field>

      <input type="file" @change="onFileChange" />

      <div id="preview">
        <img width=500px  v-if="url"  :src="url" />
      </div>

    </section>
    <footer class="modal-card-foot">
      <button class="button" type="button" @click="$parent.close()">Cerrar</button>
      <button class="button is-success" @click="addTodo">Guardar</button>
    </footer>
  </div>
</template>

<script>
import { ValidationObserver, ValidationProvider } from "vee-validate";
export default {
  name: "Añadir",
  components: {
    ValidationObserver,
    ValidationProvider
  },
  props: {
    priorities: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      pais: "",
      title: "",
      longitud: "",
      url: null
    };
  },
  methods: {
    addTodo() {
      const payload = {
        pais: this.pais,
        title: this.title,
        longitud: this.longitud,
        url: this.url
      };
      this.$emit("add-todo", payload);
    },
    onFileChange(e) {
      const file = e.target.files[0];
      this.url = URL.createObjectURL(file);
    },
  }
};
</script>

<style scoped></style>
