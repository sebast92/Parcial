<template>

  <section class="src-components-respuestas">
    
    <hr>
    <br>
    <br>

    <div class="table-responsive">
    <table class="table">
      <thead class = "thead-dark">    
      <tr>
            <th>Numero de pregunta</th>
            <th>Respuesta(s)</th>
          </tr>
        </thead>
          <tr v-for="(pregunta, index) in preguntas" :key="index">
            <td>{{ pregunta.numero }}</td>
            <td>{{ unpackArray(pregunta.respuestas) }}</td>
          </tr>
        </table>
      </div>
  </section>


</template>

<script>

export default {
  name: 'src-components-respuestas',
  props: [],
  mounted() {

  },
  data() {
    return {
      preguntas: [
        {numero: 1, respuestas: ['b', 'd']},
        {numero: 2, respuestas: ['c']},
        {numero: 3, respuestas: ['b', 'e']}
      ]
    }
  },
  methods: {
    
    unpackArray(arr) {
      var respuesta = ''
      for(var i = 0; i < arr.length; i++) {
        if(i == 0) {
          respuesta += arr[i]
        } else {
          respuesta = respuesta + ', ' + arr[i]
        }
      }
      return respuesta
    }
    
  },
  computed: {

  }
}


</script>

<style scoped lang="css">
.src-components-respuestas {

}

.jumbotron {
    background-color: lightblue;
    color: white;
  }

</style>
