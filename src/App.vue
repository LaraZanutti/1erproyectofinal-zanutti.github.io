<template>
  <div id="app">
    <NavBar
      @activarLogin="activarLogin"
      @cerrarSesion="cerrarSesion"
      @eliminarTodo="eliminarTodo"
      :usuarioLogeado="usuarioLogeado"
      :productos="carritoDeCompras"
    />
    <LoginUsuario
      v-if="mostrarLogin"
      @registrarUsuario="registrarUsuario"
      @loginUsuario="loginUsuario"
      :errorDesdeLogin="error"
    />
    <ContainerProductos
      v-if="mostrarLogin === false"
      @agregarCarrito="agregarCarrito"
      :productos="productos"
    />
  </div>
</template>

<script>
import LoginUsuario from "./components/LoginUsuario.vue";
import ContainerProductos from "./components/ContainerProductos.vue";
import NavBar from "./components/NavBar.vue";

export default {
  name: "App",
  components: {
    LoginUsuario,
    ContainerProductos,
    NavBar,
  },
  data() {
    return {
      productos: [],
      mostrarLogin: false,
      users: [],
      error: "",
      usuarioLogeado: "",
      carritoDeCompras: [],
    };
  },
  created() {
    this.traerProductos();
  },
  methods: {
    //Traigo los productos de DummyJson
    traerProductos() {
      fetch("https://dummyjson.com/products")
        .then((res) => res.json())
        .then((data) => {
          this.productos = data.products;
        });
    },
    //Muestro o escondo el componente Login
    activarLogin() {
      this.mostrarLogin = !this.mostrarLogin;
    },
    //Registrar un usuario y mostrar un toast
    registrarUsuario(user) {
      const Toast = this.$swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 2000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener("mouseenter", this.$swal.stopTimer);
          toast.addEventListener("mouseleave", this.$swal.resumeTimer);
        },
      });
      //Se agrega al array de usuarios el usuario
      this.users.push({
        username: user.username.toLowerCase(),
        password: user.password,
      });
      Toast.fire({
        icon: "success",
        title: "Te registraste correctamente",
      });
    },
    //Login de usuario
    loginUsuario(userLogin) {
      const Toast = this.$swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 2000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener("mouseenter", this.$swal.stopTimer);
          toast.addEventListener("mouseleave", this.$swal.resumeTimer);
        },
      });
      //Buscar el usuario en el array de usuarios mediante el username
      const usuarioEncontrado = this.users.find(
        (user) =>
          user.username.toLowerCase() === userLogin.username.toLowerCase()
      );
      //Si el usuario existe, comprobar si la contraseña es la correcta
      if (usuarioEncontrado) {
        if (usuarioEncontrado.password === userLogin.password) {
          this.usuarioLogeado = usuarioEncontrado.username;
          this.mostrarLogin = false;
          Toast.fire({
            icon: "success",
            title: `Bienvenido ${usuarioEncontrado.username}`,
          });
        } else {
          //La contraseña es incorrecta
          this.error = "Usuario o contraseña incorrecta";
        }
      } else {
        //El usuario no existe
        this.error = "Usuario o contraseña incorrecta";
      }
    },
    //Vuelvo a un string vacio para que los componentes hijos se den cuenta que no hay user logeado
    cerrarSesion() {
      this.usuarioLogeado = "";
    },
    agregarCarrito(producto) {
      // busco en el carrito si existe el producto
      const productoExiste = this.carritoDeCompras.find(
        (productoEnCarrito) => productoEnCarrito.id === producto.id
      );
      // si existe actualizo cantidad y subtotal
      if (productoExiste) {
        productoExiste.cantidad += producto.cantidad;
        productoExiste.subtotal += producto.subtotal;
      } else {
        // si no existe lo agrego
        this.carritoDeCompras.push(producto);
      }
    },
    //Eliminar todo del carrito de compras
    eliminarTodo() {
      this.carritoDeCompras = [];
    },
  },
};
</script>

<style>
#app {
  background-image: linear-gradient(
    to bottom,
    #b56b97,
    #77475e,
    #3d262f,
    #000000
  );
}
</style>
