<template>
    <div class="my-3">

        <label for="valueId" class="form-label">
            Asigna un valor para "n"
        </label>
        <input 
            v-model.trim="numberSerie" 
            :disabled="isInputDisabled" 
            type="text" 
            pattern="[0-9]*" 
            inputmode="numeric"
            class="form-control"
            aria-describedby="valueId" 
            placeholder="4" 
            required
        >
        <small v-if="errMsg" class="text-danger">
            {{ errMsg }}
            <br>
        </small>
        <button @click="this.disableButton(), $emit('executeSerie', parseInt(numberSerie))" :disabled="isButtonDisabled"
            type="button" class="btn-get-serie">
            Calcular la serie
        </button>

    </div>
</template>


<script>
export default {
    name: 'SeriesForm',
    props: {
        isInputDisabled: {
            type: Boolean,
            default: false,
            required: true
        },
    },
    data() {
        return {
            numberSerie: null,
            isButtonDisabled: true,
            errMsg: null
        }
    },
    watch: {
        isInputDisabled(value) {
            if (!value) {
                this.numberSerie = null
            }
        },
        numberSerie(value) {
            // general validations
            if (value == "") {
                this.errMsg = "El campo no puede estar vacio"
                this.disableButton()
            }
            else if (value < 0) {
                this.errMsg = 'El valor debe ser mayor a 0'
                this.disableButton()
            }

            else if (value % 1 != 0) {
                this.errMsg = 'El valor debe ser un número entero'
                this.disableButton()
            }
            else if (value > 40) {
                this.errMsg = '¡La cantidad no debe ser infravalorada por el consumo de la ejecución!'
                this.disableButton()
            }            
            else if (isNaN(value)) {
                this.errMsg = 'El valor debe ser un número'
                this.disableButton()
            }
            // all validations passed
            else {
                this.enableButton()
                this.errMsg = null;
            }
        }
    },
    methods: {
        disableButton() {
            this.isButtonDisabled = true;
        },
        enableButton() {
            this.isButtonDisabled = false;
        },
    }

}
</script>

<style scoped>
.btn-get-serie {
    display: block;
    margin-left: auto;
    margin-right: auto;
    margin-top: 30px;
    transition: all 0.3s ease;
    width: 100%;
}
</style>