<template>
  <section class="src-components-ingreso">
    <div class="jumbotron">
      <h2>Ingreso de gastos</h2>

      <hr>
      <br>

      <vue-form :state="formState" @submit.prevent="enviar()">

        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input type="text" id="nombre" class="form-control" autocomplete="off" v-model.trim="formData.nombre"
            name="nombre" required no-espacios entre-3-15-caracteres>

          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-2 mr-1">Campo requerido.</div>
            <div slot="entre-3-15-caracteres" class="alert alert-danger mt-2 mr-1">
              Este campo debe poseer entre {{ MinLengthNombre }} y {{ MaxLengthNombre }} caracteres para una correta
              representacion del nombre.
            </div>
            <div slot="no-espacios" class="alert alert-danger mt-2 mr-1">
              Este campo no permite espacios intermedios.
            </div>
          </field-messages>
        </validate>

        <br>

        <validate tag="div">
          <label for="descripcion">Descripcion</label>
          <input type="text" id="descripcion" class="form-control" autocomplete="off" v-model="formData.descripcion"
            name="documento" required entre-10-30-caracteres>

          <field-messages name="documento" show="$dirty">
            <div slot="required" class="alert alert-danger mt-2 mr-1">Campo requerido.</div>
            <div slot="entre-10-30-caracteres" class="alert alert-danger mt-2 mr-1">
              Este campo debe poseer entre {{ MinLengthDesc }} y {{ MaxLengthDesc }} caracteres para una descripcion
              concienzuda.
            </div>
          </field-messages>
        </validate>

        <br>

        <validate tag="div">
          <label for="importe">Importe</label>
          <input type="number" id="importe" class="form-control" autocomplete="off" v-model.number="formData.importe"
            name="importe" required mayor-a-0>

          <field-messages name="pagar" show="$dirty">
            <div slot="required" class="alert alert-danger mt-2 mr-1">Campo requerido.</div>
            <div slot="mayor-a-0" class="alert alert-danger mt-2 mr-1">
              Este campo debe poseer un valor mayor a 0, no puede haber un gasto negativo!.</div>
          </field-messages>
        </validate>

        <br>

        <button class="btn btn-success my-3 mx-3" :disabled="formState.$invalid">Enviar</button>

      </vue-form>

      <hr>
      <hr>

      <h2>Informacion sobre gastos ingresados</h2>
      
      <hr>

      <vue-form :state="formStatePrespuesto">

        <validate tag="div">
          <label for="presupuesto">Presupuesto</label>
          <input type="number" id="presupuesto" class="form-control" autocomplete="off" v-model.number="presupuesto"
            name="presupuesto" mayor-a-0 placeholder='Ingrese el presupuesto'>

          <field-messages name="presupuesto" show="$dirty">
            <div slot="mayor-a-0" class="alert alert-danger mt-2 mr-1">
              Este campo debe poseer un valor mayor a 0, no puede haber un presupuesto negativo!.</div>
          </field-messages>
        </validate>
      </vue-form>

      <hr>

      <div v-if="ingresos.length" class= "table-responsive ">
        <table class= "table-bordered">

          <tr>
            <th>Nombre</th>
            <th>Descripcion</th>
            <th>Importe</th>
            <th>Fecha</th>
          </tr>

          <tr v-for="(ingreso, index) in ingresos" :key="index">
            <td>{{ ingreso.nombre }}</td>
            <td>{{ ingreso.descripcion }}</td>
            <td>${{ ingreso.importe }}</td>
            <td>{{ ingreso.fecha }}</td>
          </tr>

          <tr
            :style="{ 'background-color': (presupuesto != null && gastoTotal() > presupuesto) ? 'red' : (gastoTotal() < 1000) ? 'green' : (gastoTotal() > 1000 && gastoTotal() < 5000) ? 'magenta' : 'orange' }">
            <td>Total</td>
            <td> {{ gastoTotal() }} </td>
          </tr>

        </table>
      </div>
      <h4 v-else class="alert alert-warning text-left">No se han cargado gastos.</h4>
    </div>

  </section>
</template>

<script>

export default {
  name: 'src-components-ingreso',
  props: [],
  mounted() {

  },
  data() {
    return {
      ingresos: [],
      formState: {},
      formStatePrespuesto: {},
      formData: this.getInitialData(),
      MinLengthNombre: 3,
      MaxLengthNombre: 15,
      MinLengthDesc: 10,
      MaxLengthDesc: 30,
      presupuesto: null
    }
  },
  methods: {

    getInitialData() {
      return {
        nombre: null,
        descripcion: null,
        importe: null
      }
    },

    agregarGasto() {
      const ingreso = {
        nombre: this.formData.nombre,
        descripcion: this.formData.descripcion,
        importe: this.formData.importe,
        fecha: new Date().toLocaleString()
      }
      this.ingresos.push(ingreso)
    },

    enviar() {
      this.agregarGasto()
      this.formData = this.getInitialData()
      this.formState._reset()
    },

    gastoTotal() {
      var gastoTotal = 0
      this.ingresos.forEach(gasto => gastoTotal += gasto.importe)
      return gastoTotal
    }
  },
  computed: {

  }
}


</script>

<style scoped lang="css">
.src-components-ingreso {

}

.jumbotron {
    background-color: #F9FEFF;
  }

  hr {
    background-color: rgb(27, 20, 20);
  }

</style>
