<template>
  <div>
    <b-table :data="ventas" default-sort="!echa">
      <b-table-column
        sortable
        searchable
        field="cliente.razon"
        label="Cliente"
        width="10"
        v-slot="props"
      >
        {{ props.row.cliente.razon }}
      </b-table-column>
      <b-table-column
        v-if="usuario.rol === 'admin'"
        sortable
        searchable
        field="regente.nombre"
        label="Regente"
        width="10"
        v-slot="props"
      >
        {{ props.row.regente.nombre }}
      </b-table-column>

      <b-table-column
        field="props.row.medicamentos[0].nombre"
        label="Medicamentos"
        width="40"
        sortable
        v-slot="props"
      >
        {{ props.row.medicamentos[0].nombre }}

        {{ props.row.medicamentos.length > 1 ? "" : "y ..." }}
      </b-table-column>
      <b-table-column
        field="fecha"
        label="Fecha"
        width="40"
        searchable
        sortable
        v-slot="props"
      >
        <span class="tag is-success">
          {{ new Date(props.row.fecha).toLocaleDateString() }}
        </span>
      </b-table-column>

      <b-table-column
        field="total"
        label="Total"
        width="40"
        searchable
        v-slot="props"
      >
        {{ props.row.total.toString() }} {{ " Bs" }}
      </b-table-column>

      <b-table-column
        v-if="usuario.rol === 'admin'"
        field="acciones"
        label="Acciones"
        width="40"
        v-slot="props"
      >
        <router-link
          :to="{
            name: 'EditarVenta',
            params: { id: props.row.id },
          }"
        >
          <b-button class="mx-2" type="is-warning"> Ver </b-button>
        </router-link>
        <b-button
          label="Eliminar"
          type="is-danger"
          @click="borrarVenta(props.row)"
        >
        </b-button>
      </b-table-column>
    </b-table>
  </div>
</template>

<script>
import { mapActions, mapState } from "vuex";
export default {
  data() {
    return {};
  },
  methods: {
    ...mapActions(["eliminarVenta", "cargarVentas"]),

    borrarVenta(fila) {
      this.$buefy.dialog.confirm({
        title: "Borrar venta",
        message:
          "Esta seguro de que desea <b>eliminar</b> la venta, esta accion no puede ser desecha.",
        confirmText: "Borrar venta",
        type: "is-danger",
        hasIcon: true,
        onConfirm: () => {
          this.$buefy.toast.open("Venta Eliminada!");
          this.eliminarVenta(fila.id);
        },
      });
    },
  },
  computed: {
    ...mapState(["ventas", "usuario"]),
  },
  created() {
    this.cargarVentas();
  },
};
</script>

<style>
</style>