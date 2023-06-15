<template>
  <div class="container d-flex flex-wrap justify-content-center gap-3 pt-4">
    <!-- Se loopean todos los productos con su productocard -->
    <ProductoCard
      @agregarCarrito="agregarCarrito"
      v-for="producto in productos"
      :key="producto.id"
      :producto="producto"
    />
  </div>
</template>

<script>
import ProductoCard from "./ProductoCard";
export default {
  name: "ContainerProductos",
  components: {
    ProductoCard,
  },
  props: {
    productos: Array,
  },
  methods: {
    agregarCarrito(producto) {
      this.$emit("agregarCarrito", producto);
    },
  },
};
</script>