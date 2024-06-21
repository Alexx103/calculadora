<template>
  <q-page padding>
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    > -->

    <div class="row justify-center">
      <div class="col-12 col-md-8">
        <q-card>
          <q-card-section class="bg-primary text-white">
            <div class="text-h6">Calculadora</div>
          </q-card-section>

          <q-card-section>
            <div class="text-h5 text-grey-5 text-right">
              {{ acomulador }} {{ actual }}
            </div>

            <div class="text-h3 text-right">
              {{ resultado }}
            </div>
          </q-card-section>

          <q-card-section class="bf-grey-4">
            <div class="row q-col-gutter-sm">
              <div class="col-3" v-for="(btn, index) in botones" 
              :key="index">
                <q-btn class="full-width text-h6" 
                  :color="noNumero(btn) ? 'indigo' : 'grey-2'" 
                  :text-color="noNumero(btn) ? 'white' : 'grey-8'" 
                  @click="btnAccion(btn)">
                  {{ btn }}
                </q-btn>
              </div>
              <div class="col-6">
                <q-btn class="full-width text--h6" color="indigo" 
                @click="reiniciar">
                  Reset
                </q-btn>
              </div>
              <div class="col-6">
                <q-btn class="full-width text--h6" color="orange"  
                @click="btnResultado">
                  =
                </q-btn>
              </div>
            </div>
          </q-card-section>
        </q-card>
        <div class="col-6">
          <div v-for="(res, index) in arrayResultado" :key="index">
            {{ res }}
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>

import { ref } from 'vue';
// import { evaluate } from 'mathjs';

export default {
  setup() {
    const botones = [7, 8, 9, "%", 4, 5, 6, "+", 1, 2, 3, "-", ".", 0, "/", "*"];
  
    const noNumero = valor => isNaN(valor); 

    const actual = ref('');
    const acomulador = ref('');
    const resultado = ref('');
    const operadorClick = ref(true);
    const arrayResultado = ref([]);


    const btnAccion = valor => {
      if(!noNumero(valor)) {
        if (operadorClick.value) {
          actual.value = '';
          operadorClick.value = false;
        }
        actual.value = `${actual.value}${valor}`;
      } else {
        ejecutarOperacion(valor);
      }
    };

    const ejecutarOperacion = valor => {
      if(valor === '.'){
        if(actual.value.indexOf('.') === -1) {
          actual.value = `${actual.value}${valor}`;
        } 
        return;
      }

      if(valor === '%') {
        if (actual.value !== '') {
          actual.value = `${parseFloat(actual.value) / 100}`;
          return;
        }
      }

      if (valor === '-' && valor === '') {
        actual.value = '-';
        return;
      }
        agregamosOperador(valor);
      };

      const agregamosOperador = valor => {
        if (!operadorClick.value) {
          acomulador.value += `${actual.value} ${valor} `;
          actual.value = '';
          operadorClick.value = true;
        }
      };

      const reiniciar = () => {
        actual.value = '';
        acomulador.value = '';
        resultado.value = '';
        operadorClick.value = false;
        arrayResultado.value = [];
      };

      const btnResultado = () => {
        try {
          resultado.value = eval(acomulador.value + actual.value);
          arrayResultado.value.push(`${acomulador.value} + ${actual.value} = ${resultado.value}`);
        } catch(e) {
          resultado.value = 'Error';
        }
      };
 
      return{
        botones,
        noNumero,
        btnAccion,
        actual,
        acomulador,
        reiniciar,
        btnResultado,
        resultado,
        operadorClick,
        arrayResultado
      };

    }
}; 

</script>

<style scoped>
.text-h5 {
  height: 23px;
}
.text-h3 {
  height: 50px;
}
</style>
