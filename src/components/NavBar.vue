<template>
  <div>
    <b-sidebar
      id="sidebar-right"
      title="Carrito de compras"
      shadow
      width="50%"
      ref="carritoSidebar"
    >
      <div class="px-3 py-2">
        <CarritoCompras
          @eliminarTodo="eliminarTodo"
          @activarLogin="activarLogin"
          @esconderSidebar="esconderSidebar"
          :productos="productos"
          :usuarioLogeado="usuarioLogeado"
        />
      </div>
    </b-sidebar>
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand href="#" class="lariz">LariZtore</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" class="justify-content-end p-3 ml-4" is-nav>
        <b-navbar-nav>
          <b-nav-item v-b-toggle.sidebar-right>
            <b-button class="carrito">
              <b-icon-cart4 />
              <span class="cantidadProductos">{{ productos.length }}</span>
            </b-button>
          </b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav>
          <b-button
            class="button"
            @click="activarLogin"
            v-if="usuarioLogeado.length === 0"
            ><b-icon-person-circle />&nbsp;Login</b-button
          >
          <div v-if="usuarioLogeado.length > 0">
            <b-button class="button" @click="easterEgg"
              ><b-icon-person-circle />&nbsp;{{ usuarioLogeado }}</b-button
            >
            <b-button class="button" @click="cerrarSesion"
              ><b-icon-x-square-fill variant="danger" />&nbsp;Cerrar
              sesión</b-button
            >
          </div>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import CarritoCompras from "./CarritoCompras.vue";
export default {
  components: {
    CarritoCompras,
  },
  props: {
    usuarioLogeado: String,
    productos: Array,
  },
  methods: {
    //Emitir activarLogin al app.vue
    esconderSidebar() {
      this.$refs.carritoSidebar.hide();
    },
    activarLogin() {
      this.$emit("activarLogin");
    },
    //Emitir cerrarSesion al app.vue
    cerrarSesion() {
      this.$emit("cerrarSesion");
    },
    //Emitir eliminarTodo al app.vue
    eliminarTodo() {
      this.$emit("eliminarTodo");
    },
    //chimi
    easterEgg() {
      this.$swal.fire({
        html: '<a style="font-size: 30px; color: pink;" href="https://github.com/LaraZanutti" target="_blank">https://github.com/LaraZanutti</a>',
        width: 600,
        padding: "3em",
        showConfirmButton: false,
        color: "#716add",
        background: "#fff url(/images/trees.png)",
        backdrop: `
    rgba(0,0,123,0.4)
    url("https://sweetalert2.github.io/images/nyan-cat.gif")
    left top
    no-repeat
  `,
      });
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap");
.lariz {
  color: rgb(244, 147, 215);
  font-family: "Bebas Neue", sans-serif;
  font-size: 40px;
  padding-left: 20px;
}
.button {
  color: rgb(244, 147, 215);
  background: rgba(0, 0, 0, 0);
  border: 1px solid rgb(244, 147, 215);
  margin-right: 10px;
  margin-left: 10px;
}

.button:hover {
  background: rgb(244, 147, 215);
  color: white;
}

.carrito {
  color: rgb(244, 147, 215);
  background: rgba(0, 0, 0, 0);
  border: 1px solid rgb(244, 147, 215);
  margin-left: 10px;
  display: flex;
  min-width: 70px;
  gap: 10px;
}

.carrito:hover {
  background: rgb(244, 147, 215);
  color: white;
}

.botonComprar {
  font-size: 20px;
  font-weight: 500;
  display: inline-block;
}

.cantidadProductos {
  min-width: 20px;
  min-height: 20px;
  background: rgb(244, 147, 215);
  color: white;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 3px;
}
</style>