<template>
  <div>
    <nav>
      <div>
<img src="../assets/river.png" alt="">
<h1>River World</h1>
      </div>
</nav>
    <section class="section">
      <div class="container">
        <div class="level">
        <h1>Numero de rios <strong>{{todos.length}}</strong></h1>
          
          <b-button class="is-success" @click="isAddModalActive = true">
            Añadir
          </b-button>
        </div>

        <b-table :data="todos" default-sort="priority">
          <template slot-scope="todos">
             <b-table-column field="Foto" label="Foto">
              <img :src="todos.row.url" width="150px ">
            </b-table-column>

            <b-table-column field="todo" label="Nombre" sortable>
              {{ todos.row.todo }}
            </b-table-column>

            <b-table-column field="pais" label="Pais" sortable>
              {{ todos.row.pais }}
            </b-table-column>

             <b-table-column field="longitud" label="Longitud" sortable numeric>
              {{ todos.row.longitud }} Km
            </b-table-column>       


            <b-table-column label="Editar">
              <b-button
                type="is-text"
                @click="openEditModal(todos.row)"
              >Editar</b-button>
            </b-table-column>

            <b-table-column label="Borrar">
              <b-button
                type="is-text"
                @click="deleteTodo(todos.row)"
              >Borrar</b-button>
            </b-table-column>
          </template>
        </b-table>
      </div>

    </section>

    <b-modal :active.sync="isEditModalActive" has-modal-card>
      <editar
        :todo="selectedTodo"
        :priorities="priorities"
        @edit-todo="onEditTodo"
      ></editar>
    </b-modal>

    <b-modal :active.sync="isAddModalActive" has-modal-card>
      <anadir
        @add-todo="onAddTodo"
        :priorities="priorities"
      ></anadir>
    </b-modal>
  </div>
  
</template>

<script>
import Editar from "@/components/Editar";
import Anadir from "@/components/Anadir";

export default {
  name: "TodoTable",
  components: { Editar, Anadir },
  data() {
    return {
      initialTodos: [
        {
          todo: "Miño",
          pais: "España",
          longitud:'350',
          url: require('../../src/assets/Miño.jpg')
          
        },
        {
          todo: "Ebro",
          pais: "España",
          longitud:'930',
          url: require('../../src/assets/ebro.png')
        },
        {
          todo: "Misisipi ",
          pais: "Estados Unidos",
          longitud:'3.778',
          url: require('../../src/assets/misisipi.jpg')
        },
        {
          todo: "Tamesis",
          pais: "Inglaterra",
          longitud:'346',
          url: require('../../src/assets/tamesis.jpg')
        }
      ],
      todos: [],
      priorities: [
        {  name: "España" },
        {  name: "Francia" },
        {  name: "Portugal" },
        {  name: "Andorra" }
      ],
      isEditModalActive: false,
      selectedTodo: {},
      isAddModalActive: false
    };
  },
  mounted() {
    if (localStorage.getItem("todos")) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    } else {
      this.todos = this.initialTodos;
    }
  },
  methods: {
    openEditModal(todo) {
      this.selectedTodo = todo;
      this.isEditModalActive = true;
    },
    onAddTodo(item) {
      // get the highest number id to iterate on it
      const highestId = Math.max.apply(Math, this.todos.map(item => item.id));
      // Add the item to the array

      this.$buefy.dialog.confirm({
        title: `Añadir ` + item.title ,
        cancelText:'Cancelar',
        confirmText: "Guardar",
        type: "is-success",
        hasIcon: true,
        message: `Confirmar ` + 'Pais ' + item.pais,
        onConfirm: () => {
          // find in the array and remove
          const index = this.todos.indexOf(item);
            this.todos.push({
              id: highestId + 1,
              todo: item.title,
              pais: item.pais,
              longitud: item.longitud,
              url: item.url
      });
          // save the updated array in localstorage
          this.saveLocalStorageTodos();
        }
      });

    
      // save the updated array in localstorage
      this.saveLocalStorageTodos();
      this.isAddModalActive = false;
    },
    onEditTodo(item) {
      const todo = this.findTodo(item);
      // Apply the updated values
      todo.todo = item.todo;
      todo.pais = item.pais;
      todo.longitud = item.longitud;
      todo.url = item.url;
      // save the updated array in localstorage
      this.saveLocalStorageTodos();
      // close the modal
      this.isEditModalActive = false;
    },
    deleteTodo(item) {
      this.$buefy.dialog.confirm({
        title: `Borrar ` + item.todo ,
        cancelText:'Cancelar',
        confirmText: "Borrar",
        type: "is-danger",
        hasIcon: true,
        message: `¿Seguro quieres borrar?`,
        onConfirm: () => {
          // find in the array and remove
          const index = this.todos.indexOf(item);
          this.todos.splice(index, 1);
          // save the updated array in localstorage
          this.saveLocalStorageTodos();
        }
      });
    },
    deleteAllTodos() {
      this.$buefy.dialog.confirm({
        title: `Borrar todos`,
        cancelText:'Cancelar',
        confirmText: "Borrar Todos",
        type: "is-danger",
        hasIcon: true,
        message: `¿Seguro quieres borrar todos?`,
        onConfirm: () => {
          this.todos = [];
          // save the updated array in localstorage
          this.saveLocalStorageTodos();
        }
      });
    },
    findTodo(item) {
      return this.todos.find(todo => todo.id === item.id);
    },
    saveLocalStorageTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  }
};
</script>

<style lang="scss" scoped>
nav{
border-bottom: 1px solid gray;
}
nav div{
display: flex;
max-width: 1344px;
margin: 10px auto;
}
nav h1 , nav img{
  margin: 0 10px;
}
</style>
