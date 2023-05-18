<template>
    <div class="grid">
        <div class="col-12">
            <div class="card p-fluid">
                <h5>DATOS HISTORICOS DEL MERCADO DE VALORES</h5>
                <h5>Lyon, Francia</h5>
                <div class="field">
                        <Chart type="line" :data="chartData" :options="options" class="h-30rem"></Chart>
                </div>
                <br>
            </div>
        </div>

        <div class="col-12">
            <div class="card p-fluid">
                <h5>CAMBIO PORCENTUAL POR DIA DE ACCIONES APPL (2017-01-05 / 2017-02-05)</h5>
                <div class="field">
                    <DataTable :value="aapl" showGridlines paginator :rows="3"
                        :rowsPerPageOptions="[3,5,10]" tableStyle="min-width: 50rem">
                        <Column field="date" header="FECHA"></Column>
                        <Column field="open" header="VALOR APERTURA"></Column>
                        <Column field="close" header="VALOR CIERRE"></Column>
                    </DataTable>
                </div>
            </div>
        </div>

    </div>
</template>

<script>

import axios from "axios";

export default {
    data() {
        return {
            aapl: [],
            opciones: [],
            datos: [],
            chartData: {
                labels: [],
                datasets: [
                    {
                        label: 'CAMBIO PORCENTUAL TRIMESTRAL',
                        data: [],
                        fill: false,
                        backgroundColor: '#2f4860',
                        borderColor: '#2f4860',
                        tension: .4
                    }
                ]
            },

            options: {
                responsive: true,
            }
        };

    },
    async mounted() {
        axios.get("https://eodhistoricaldata.com/api/eod/MCD.US?from=2017-01-05&to=2017-02-05&period=d&fmt=json&api_token=OeAFFmMliFG5orCUuwAKQ8l4WWFQ67YX").then((res) => {
            this.aapl = res.data;
            for (var i = 0; i < this.aapl.length; i++) {
                var counter = this.aapl[i];
                this.opciones.push(counter.date);
                this.datos.push(counter.close);
            }
            this.chartData.labels = this.opciones;
            this.chartData.datasets[0].data = this.datos;

        })
    }
};
</script>
