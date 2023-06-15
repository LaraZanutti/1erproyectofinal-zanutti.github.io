<template>
  <div>
    <b-card
      :title="producto.title"
      :img-src="producto.thumbnail"
      img-alt="Image"
      img-top
      tag="article"
      class="mb-2 card"
    >
      <b-card-text class="precio">
        {{ `$ ${producto.price}` }}
      </b-card-text>

      <div>
        <b-input-group class="mt-3">
          <b-input-group-prepend>
            <b-button @click="restarProducto" variant="outline-danger"
              ><b-icon-patch-minus
            /></b-button>
          </b-input-group-prepend>
          <b-form-input
            class="text-center"
            v-model="cantidadProducto"
          ></b-form-input>
          <b-input-group-append>
            <b-button @click="sumarProducto" variant="outline-success"
              ><b-icon-patch-plus
            /></b-button>
          </b-input-group-append>
        </b-input-group>
      </div>
      <b-button
        :disabled="cantidadProducto === 0"
        class="mt-4"
        @click="agregarCarrito"
        variant="success"
        >Agregar al carrito</b-button
      >
    </b-card>
  </div>
</template>

<script>
export default {
  name: "ProductoCard",
  data() {
    return {
      cantidadProducto: 0,
    };
  },
  methods: {
    restarProducto() {
      if (this.cantidadProducto === 0) return;
      this.cantidadProducto--;
    },
    sumarProducto() {
      this.cantidadProducto++;
    },
    agregarCarrito() {
      this.$emit("agregarCarrito", {
        id: this.producto.id,
        producto: this.producto.title,
        cantidad: this.cantidadProducto,
        precio: this.producto.price,
        subtotal: this.cantidadProducto * this.producto.price,
      });
    },
  },
  props: {
    producto: Object,
  },
};
</script>

<style scoped>
.precio {
  font-size: 30px;
  font-weight: 600;
  margin-bottom: 25px;
}
.card {
  width: 300px;
  text-align: center;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 16px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(8.4px);
  -webkit-backdrop-filter: blur(8.4px);
}

.card-img-top {
  height: 15vw;
  object-fit: cover;
  width: 100%;
}
</style>