<template>
  <div class="py-5">
    <div class="justify-content-center">
      <div class=" w-75 card">
        <div class="card-section bg-primary">
          <div class="card-body text-white text-center">
            <h2>Calculadora</h2>
          </div>
        </div>
        <div class="card-section">
          <div class="text-secondary sentenciaClass">
            <h6>{{acumulador + actualNumber}}</h6>
          </div>
          <div class="text-right resultadoClass">
            <strong>{{resultado}}</strong>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
        <div class="col-sm-3 py-1 " v-for="n in botones" :key="n">
            <p class="bgButtons text-center py-1" @click="btnAccion(n)">{{n}}</p>

        </div>
        <div class="col-sm-3 py-1">
            <p class="bg-warning bgOperaciones text-white text-center py-3" @click="btnResultado">=</p>

        </div>
        <div class="col-sm-3 py-1">
            <p class="bg-primary bgOperaciones text-white text-center py-3" @click="btnReset">CE</p>

        </div>

    </div>
  </div>
</template>

<script>
import {ref} from 'vue'
import {evaluate,round} from 'mathjs'
export default {
    setup(){
        const botones =[7,8,9,"/",4,5,6,"*",1,2,3,"-",".",0,"+"]
        const notIsNumber = valor => isNaN(valor)
        const actualNumber = ref('')
        const acumulador =ref('')
        const resultado =ref('')
        const operadorClick = ref(true)
        const ejecutarOperacion = valor=>{
            if(valor==="."){
                if(actualNumber.value.indexOf('.')===-1){
                    actualNumber.value = `${actualNumber.value}${valor}`
                }
                return
            }
             agregarOperador(valor)
        }
        const agregarOperador =(valor)=>{
            if(!operadorClick.value){
                acumulador.value += `${actualNumber.value} ${valor} `
            actualNumber.value=''
            operadorClick.value=true

            }
            
            

        }
        const btnAccion =(valor)=>{
            
            if(!notIsNumber(valor)){
                if(operadorClick.value){
                actualNumber.value=''
                operadorClick.value=false

            }
                actualNumber.value = `${actualNumber.value}${valor}`
            }else{
                ejecutarOperacion(valor)

            }
           

        }
        const btnReset = ()=>{
            actualNumber.value=''
            acumulador.value=''
            resultado.value=''
            operadorClick.value=true
        }
        const btnResultado =()=>{
            if(!operadorClick.value){
                resultado.value= evaluate(acumulador.value + actualNumber.value)

            }
            else{
                resultado.value ='ERROR...'
            }
            

        }
        return{
            botones,
            btnAccion,
            actualNumber,
            acumulador,
            btnReset,
            btnResultado,
            resultado
        }
    }
};
</script>

<style scoped>
.bgButtons{
    background: #5585b5;
    cursor: pointer;
    color: whitesmoke;
}
.bgButtons:hover{
    background: #bbe4e9;
}
.bgOperaciones{
    cursor: pointer;
}
.sentenciaClass{
    height: 25px;
}
.resultadoClass{
    height: 50px;
}
</style>
