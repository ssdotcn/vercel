<template>
    <div class="grid">
        <div class="col-12">

            <div class="card p-fluid">
                <h5>EMPLEADO API REST</h5>

                <div class="formgrid grid">
                    <div class="field col-1">
                        <label for="id" class="mr-2">ID</label>
                        <InputNumber id="id" v-model="datosBuscar.idEmpleado" type="numeric" />
                    </div>
                </div>

                <div class="formgrid grid">
                    <div class="field col-6">
                        <label for="nom">NOMBRE(S)</label>
                        <InputText id="nom" v-model="empleado.employee_name" type="text" readonly/>
                    </div>
                </div>

                <div class="formgrid grid">
                    <div class="field col">
                        <label for="sal">SALARIO</label>
                        <InputNumber id="sal" v-model="empleado.employee_salary" type="numeric" mode="currency" currency="USD" locale="en-US" readonly/>
                    </div>
                    <div class="field col">
                        <label for="ed">EDAD</label>
                        <InputNumber id="ed" v-model="empleado.employee_age" type="numeric" readonly/>
                    </div>
                </div>

                <div class="field">
                    <Button type="button" label="CONSULTAR DATOS" severity="success" :disabled="disabledBtn" :loading="false" @click="obtenerDatosID"/>
                </div>

            </div>

            <div class="card">
                <h5>LISTA EMPLEADOS</h5>

                <DataTable :value="empleados" showGridlines paginator :rows="3"
                        tableStyle="min-width: 50rem">
                    <Column field="employee_name" header="NOMBRE"></Column>
                    <Column field="employee_salary" header="SALARIO"></Column>
                    <Column field="employee_age" header="EDAD"></Column>
                </DataTable>

            </div>
            <Toast/>
        </div>
    </div>

</template>

obtenerDatos
<script>
    import axios from "axios";

    export default {
        data() {

            return {

                datosBuscar: {
                    idEmpleado: null,
                },

                datosBusqueda: {
                    nombreEmpleado: null,
                    salarioEmpleado: null,
                    edadEmpleado: null,
                },

                empleados: [],

                empleado: {},

                jsjs: {},
            }
        },

        created() {

        },


        computed: {
            disabledBtn() {
                if (this.datosBuscar.idEmpleado > 0) {
                    return false;
                } else {
                    return true;
                }
            },


        },

        mounted() {
            this.obtenerDatos();

        },

        methods: {

            obtenerDatos() {

                let url = "https://dummy.restapiexample.com/api/v1/employees";

                console.log(url);

                axios.get(url, { headers: {'Access-Control-Allow-Origin': '*',}}).then((response) => {this.empleados = response.data.data;}).catch((error) => {console.log(error);});

                // axios.get(url).then((res) => (this.empleados = res.data))

                // const res = axios.get("https://dummy.restapiexample.com/api/v1/employees/");
                // this.empleados = res.data;

            },

            obtenerDatosID() {

                let url = "https://dummy.restapiexample.com/api/v1/employee/"+ String(this.datosBuscar.idEmpleado);

                console.log(url);

                axios.get(url,{ headers: {'Access-Control-Allow-Origin': '*',}}).then((response) => {this.empleado = response.data.data; this.$toast.add({ severity: 'info', summary: 'CONFIRMACION', detail: 'DATOS OPTENIDOS', life: 3000 }); }).catch((error) => {console.log(error); this.$toast.add({ severity: 'error', summary: 'RECHAZO', detail: 'ERROR AL CONSULTAR EN LA API', life: 3000 }); });

                // axios.get(url).then((res) => (this.empleado = res.data))

                // console.log(this.empleado.data.employee_name);

                // for (var i = 0; i < this.empleado.length; i++) {
                //     this.datosBusqueda.nombreEmpleado = this.empleado[i].employee_name;
                //     this.datosBusqueda.salarioEmpleado = this.empleado[i].employee_salary;
                //     this.datosBusqueda.edadEmpleado = this.empleado[i].employee_age;
                // }

                //this.pasarDatosID();
            },

            pasarDatosID() {

                this.datosBusqueda.nombreEmpleado = this.empleado.employee_name;
                this.datosBusqueda.salarioEmpleado = this.empleado.employee_salary;
                this.datosBusqueda.edadEmpleado = this.empleado.employee_age;

            },

        },
    }



</script>


<!-- <template>
    <div class="grid">
        <div class="col-12">

            <div class="card p-fluid">
                <h5> EMPLEADO API REST</h5>


                <div class="field">
                    <label for="nom">NOMBRE(S)</label>
                    <InputText id="nom" v-model="nom" type="text" />
                </div>

                <div class="field">
                    <label for="sal">SALARIO</label>
                    <InputText id="sal" v-model="app" type="text" />
                </div>

                <div class="field">
                    <label for="ed">EDAD</label>
                    <InputText id="ed" v-model="apm" type="text" />
                </div>

                <div class="field">
                    <Button type="button" label="RFC" severity="secondary" :disabled="" :loading="" @click=""/>
                </div>



                <div class="p-fluid formgrid grid">
                    <div class="field col-12">
                        <label for="nombre">Nombre:</label>
                        <InputText id="nombre" type="text" :value="empleado.employee_name" readonly />
                    </div>
                    <div class="field col-12">
                        <label for="salario">Salario:</label>
                        <InputText id="salario" type="text" :value="empleado.employee_salary" readonly />
                    </div>
                    <div class="field col-12">
                        <label for="edad">Edad:</label>
                        <InputText id="edad" type="text" :value="empleado.employee_age" readonly />
                        <br>
                    </div>
                    <div class="field col-12">
                        <label for="number-input">ID Empleado</label>
                        <InputNumber id="number-input" v-model="id_emp" />
                    </div>
                    <div class="field col-12 md:col-6 ">
                        <Button label="Consultar Datos" class="p-button-success mr-2 mb-2" icon="pi pi-search"
                            @click="Consultar()" v-tooltip="'Click al presionar'"></Button>
                    </div>
                </div>
            </div>

        </div>
    </div>


    <div class="grid">
        <div class="col-12">
            <div class="card">
                <div>
                    <h2>Lista de empleados</h2>
                    <DataTable v-bind="value" :value="empleados" showGridlines paginator :rows="3"
                        :rowsPerPageOptions="[3, 10, 15, 24]" tableStyle="min-width: 50rem">
                        <Column field="employee_name" header="Nombre"></Column>
                        <Column field="employee_salary" header="Salario"></Column>
                        <Column field="employee_age" header="Edad"></Column>
                    </DataTable>
                </div>
            </div>
        </div>
    </div>
</template>
<script> -->
//import axios from "axios";

/*
export default {
    data() {
        return {
            empleados: [],
            id_emp: null,
            message: "No has ingresado su ID del empleado",
            empleado: {}
        };

    },
    mounted() {
        axios.get("https://dummy.restapiexample.com/api/v1/employees", {
            headers: {
                'Access-Control-Allow-Origin': '*',
            }
        }).then((response) => {
            this.empleados = response.data.data;
        })
            .catch((error) => {
                console.log(error);
            });
    },

    methods: {
        Consultar() {

            if (this.id_emp === null) {
                alert(this.message);

            } else {
                axios.get(`https://dummy.restapiexample.com/api/v1/employee/${this.id_emp}`, {
                    headers: {
                        'Access-Control-Allow-Origin': '*',
                    }
                }).
                    then((response) => {
                        this.empleado = response.data.data;
                    }).catch((error) => {
                        alert("Error: " + error.response.status + " " + error.response.statusText);
                        console.log(error);
                    });
            }


        }
    }
};*/
<!-- </script> -->
