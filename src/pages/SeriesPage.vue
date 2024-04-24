<template>
    <div class="center">
        <div id="errorAlert"></div>

        <div class="row justify-content-center">
            <div class="col-11">
                <div class="card p-4 rounded" id="container-components">
                    <div class="row">
                        <div class="col-12">
                            <SeriesForm 
                                :isInputDisabled="isInputDisabled" 
                                @execute-serie="solveSerie" 
                            />

                            <br><br>

                            <SeriesResults 
                                :showResults="showResults" 
                                :result="serieResult" 
                            />
                        </div>
                        <div v-if="tryAgain" class="col-12">
                            <button type="button" class="try-again" @click="resetApp">
                                Reintentar
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import Series from '@/classes/Serie.class'
import SeriesForm from '@/components/SeriesForm'
import SeriesResults from '@/components/SeriesResults'

export default {
    name: 'SeriesPage',
    components: {
        SeriesForm,
        SeriesResults
    },
    data() {
        return {
            showError: false,
            isInputDisabled: false,
            showResults: false,
            serieResult: {},
            tryAgain: false,
        }
    },
    computed: {
        serieInstance() {
            return new Series();
        }
    },
    methods: {
        solveSerie(numberSerie) {
            try {

                this.serieInstance.setNumber = numberSerie

                this.serieResult = {
                    result: this.serieInstance.getSerie(),
                    numberSerie
                };

                this.showResults = true

                this.isInputDisabled = true
                this.tryAgain = true

            } catch (error) {
                this.createErrorAlert(error)
            }
        },
        resetApp() {
            this.tryAgain = false
            this.showResults = false
            this.serieResult = {}
            this.isInputDisabled = false
        },
        createErrorAlert(error) {
            const alert = document.createElement('div')
            alert.className = 'alert alert-danger alert-dismissible fade show'
            alert.setAttribute('role', 'alert')
            alert.innerHTML = `
                    <strong>Opss!</strong> Recuerda que ${error.message}
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                `
            document.getElementById('errorAlert').appendChild(alert)

            this.isInputDisabled = false
        }

    },
}
</script>