<template>
  <div class="mt-3">
    {{ clienteSeleccionado }}
    <b-button
      label="Borrar Seleccionados"
      type="is-danger"
      icon-left="close"
      class="field"
      @click="eliminarSeleccionados"
    />
    <b-table
      :data="medicamentos"
      :focusable="true"
      :mobile-cards="true"
      checkable
      :checked-rows.sync="checkedRows"
    >
      <b-table-column
        field="nombre"
        label="Nombre del Medicamento"
        numeric
        v-slot="props"
      >
        {{ props.row.nombre }}
      </b-table-column>

      <b-table-column field="cantidad" numeric label="Cantidad" v-slot="props">
        {{ props.row.cantidad }}
      </b-table-column>

      <b-table-column field="precio" label="Precio" numeric v-slot="props">
        {{ props.row.precio }}
      </b-table-column>

      <b-table-column field="total" label="Total" numeric v-slot="props">
        {{ props.row.precio * props.row.cantidad }} {{ "Bs" }}
      </b-table-column>

      <b-table-column
        field="vencimiento"
        label="Fecha Vencimiento"
        centered
        v-slot="props"
      >
        <span class="tag is-success">
          {{ new Date(props.row.vencimiento).toLocaleDateString() }}
        </span>
      </b-table-column>

      <b-table-column label="Lote" v-slot="props">
        {{ props.row.lote }}
      </b-table-column>

      <template #footer>
        <div class="has-text-right">
          <div class="mt-3">
            {{ "Precio Total:" }} {{ calcularTotal() }} {{ " Bs" }}
          </div>
          <!-- <div class="mt-3 is-align-content-end title is-4">
            {{ "Precio Final:" }}
            {{ calcularTotal() - calcularTotal() * (descuento / 100) }}
            {{ " Bs" }}
          </div> -->
        </div>
      </template>
    </b-table>

    <!-- <div>
      <b-field label="Descuento">
        <b-slider
          v-model="descuento"
          lazy
          :step="5"
          :min="0"
          :max="30"
          size="is-medium"
          tooltip-always
          ticks
          :custom-formatter="val => val + '%'"
        >
        </b-slider>
      </b-field>
    </div> -->
  </div>
</template>

<script>
import { mapActions, mapState } from "vuex";

export default {
  props: {
    clienteSeleccionado: {
      type: Object,
    },
  },
  methods: {
    ...mapActions(["actualizarMedicamentoLista"]),
    eliminarSeleccionados() {
      this.actualizarMedicamentoLista(this.checkedRows);
      this.checkedRows = [];
    },
    calcularTotal() {
      const totales = this.medicamentos.map((med) => {
        return med.cantidad * med.precio;
      });
      let suma = 0;
      totales.forEach((element) => {
        suma += element;
      });

      return suma;
    },
  },
  data() {
    return {
      checkedRows: [],
      descuento: 0,
    };
  },
  computed: {
    ...mapState(["medicamentos", "venta"]),
  },
};
</script>

<style>
</style>