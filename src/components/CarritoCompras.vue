<template>
  <div>
    <div v-if="productos.length > 0">
      <b-table class="text-center" striped hover :items="productos"></b-table>
      <div class="d-flex justify-content-center flex-column">
        <b-button variant="outline-danger" class="mt-3" @click="eliminarTodo"
          ><b-icon-cart-x />&nbsp;Eliminar productos del carrito</b-button
        >
        <b-button
          variant="outline-success"
          class="mt-3"
          @click="finalizarCompra"
          ><b-icon-cart-plus />&nbsp;Finalizar compra</b-button
        >
      </div>
      <p class="total">Total: ${{ calcularTotal }}</p>
    </div>
    <div v-else class="noHayNada">
      <img
        src="https://media.tenor.com/lx2WSGRk8bcAAAAC/pulp-fiction-john-travolta.gif"
        alt=""
      />
      <p>No hay nada en el carrito :(</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    productos: Array,
  },
  methods: {
    eliminarTodo() {
      this.$emit("eliminarTodo");
    },
    finalizarCompra() {
      this.$swal("Próximamente");
    },
  },
  computed: {
    //Se usa la computada para calcular el total
    calcularTotal() {
      let total = 0;
      this.productos.forEach((producto) => {
        total += producto.subtotal;
      });
      return total;
    },
  },
};
</script>

<style scoped>
.total {
  font-size: 40px;
  font-weight: bold;
  text-align: center;
  padding: 20px;
  color: rgb(214, 115, 115);
}

.noHayNada {
  font-size: 30px;
  font-weight: bold;
  text-align: center;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  gap: 30px;
}

.noHayNada img {
  width: 50%;
}
</style>