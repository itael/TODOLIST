<template>
  <div class="row align-self-center" v-if="usuario == ''">
    <label clas="input-group-text">Ingrese su Nombre</label>
    <input class="input-group-text" type="text" v-model="nuevoUsuario" />
    <button class="btn btn-success" v-on:click="addUsuario">
      Ingresar tareas
    </button>
  </div>
  <div v-else>
    <h1 class="bg-primary text-white text-center p-2">
      Lista de tareas de {{ usuario }}
    </h1>
    <div class="container-fluid p-4">
      <div class="row" v-if="filtroTareas.length == 0">
        <div class="col text-center">
          <b>Bravo!! has completado todas tus tareas</b>
        </div>
      </div>
      <div v-else>
        <div class="row">
          <div class="col font-weight-bold">Tarea</div>
          <div class="col-2 font-weight-bold">Terminada</div>
        </div>
        <div class="row" v-for="t in filtroTareas" v-bind:key="t.descripcion">
          <div class="col">{{ t.descripcion }}</div>
          <div class="col-2 text-center">
            <input
              type="checkbox"
              v-model="t.terminada"
              class="form-check-input"
            />
            {{ t.terminada }}
          </div>
        </div>
      </div>

      <div class="row py-2">
        <div class="col">
          <input type="text" class="form-" v-model="nuevoItem" />
        </div>
        <div class="col-2">
          <button class="btn btn-primary" v-on:click="addTarea">Agregar</button>
        </div>
      </div>

      <div class="row bg-dark py-2 mt-4 text-white justify-content-between">
        <div class="col text-center">
          <input
            type="checkbox"
            class="form-check-input"
            v-model="banderaCompletada"
          />
          <label for="" class="form-check-label font-weight-bold">
            Ocultar tareas terminadas
          </label>
        </div>
        <div class="col text-center">
          <button class="btn btn-sm btn-warning" v-on:click="deleteTarea">
            Eliminar tareas
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      usuario: "",
      nuevoUsuario: "",
      tareas: [],
      banderaCompletada: false,
      nuevoItem: "",
    };
  },
  computed: {
    filtroTareas() {
      return this.banderaCompletada
        ? this.tareas.filter((t) => !t.terminada)
        : this.tareas;
    },
  },
  methods: {
    addTarea() {
      this.tareas.push({
        descripcion: this.nuevoItem,
        terminada: false,
      });
      this.guardarTareas();
      this.nuevoItem = "";
    },
    addUsuario() {
      this.usuario = this.nuevoUsuario;
      localStorage.setItem("usuario", JSON.stringify(this.nuevoUsuario));
    },
    guardarTareas() {
      localStorage.setItem("tareas", JSON.stringify(this.tareas));
    },
    deleteTarea() {
      this.tareas = this.tareas.filter((t) => !t.terminada);
      this.guardarTareas();
    },
  },
  created() {
    let data = localStorage.getItem("tareas");
    if (data != null) {
      this.tareas = JSON.parse(data);
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
