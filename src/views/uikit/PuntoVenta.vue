<template>

    <div class="p-grid">
        <div class="p-col-12">
            <div class="card">
                <Panel header="PUNTO DE VENTA" style="height: 100%">
                    <Toolbar class="p-mb-4">
                        <template v-slot:start>
                            <InputText type="text" size="40" placeholder="NOMBRE DEL PRODUCTO..." v-model="productoItem.nombreProducto" class="mr-2"/>

                            <InputNumber type="numeric" placeholder="CANTIDAD" v-model="productoItem.cantidadProducto" :min="1" class="mr-2"/>
                            <!-- <InputText type="text" placeholder="Cant" v-model="productoItem.cantidad" class="mr-2"/> -->
                            <!-- InputNumber -->
                            <InputNumber type="numeric" placeholder="$ PRECIO" v-model="productoItem.precioUnitario" mode="currency" currency="USD" locale="en-US" class="mr-2" />
                            <!-- <InputText type="text" placeholder="$ Precio U." v-model="productoItem.precioUnitario" class="mr-2"/> -->
                        </template>
                        <template v-slot:end>
                            <Button label="Registrar" icon="pi pi-plus" class="p-button-success p-mr-2" :disabled="disabledBtnR" @click="registrarCompra"/>
                        </template>
                    </Toolbar>
                    <br>

                    <DataTable :value="tablaCompras" class="p-datatable-gridlines" dataKey="cns" :rows="5"
                        paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown"

                        currentPageReportTemplate="Visualizando {last} de {totalRecords} productos"
                        style="margin-bottom: 20px" :paginator="true" responsiveLayout="scroll">

                        <!-- :rowsPerPageOptions="[5,10,25]" -->

                        <Column field="idP" header="ID" :sortable="true" style="width:50px"></Column>
                        <Column field="nombreP" header="NOMBRE DEL PRODUCTO" style="width:370px;text-align:center"></Column>
                        <Column field="precioP" header="PRECIO" dataType="numeric" mode="currency" currency="USD" locale="en-US" style="width:80px;text-align:center">
                            <template #body="slotProps">
                                    {{ formatoMoneda(slotProps.data.precioP) }}
                            </template>
                        </Column>
                        <Column field="cantidadP" header="CANTIDAD" style="width:60px;text-align:center"></Column>
                        <Column field="importeP" header="IMPORTE" style="width:80px;text-align:center">
                            <template #body="slotProps">
                                {{ formatoMoneda(slotProps.data.importeP) }}
                            </template>
                        </Column>
                        <Column header="ACCIONES" style="width:120px;text-align:center">
                            <!-- <template style="text-align:center" #header>
                                ACCIONES
                            </template> -->
                            <template #body="slotProps">
                                <div class="justify-content-center">
                                    <Button icon="pi pi-pencil" type="button" class="p-button-success p-mr-2 p-mb-1 mr-3" @click="editarProductos(slotProps.data)"></Button>
                                    <Button icon="pi pi-trash" type="button" class="p-button-danger p-mb-1" @click="confirmaEliminarProductos(slotProps.data)"></Button>
                                </div>
                                <!-- <Button icon="pi pi-pencil" type="button" class="p-button-success p-mr-2 p-mb-1 mr-2" @click=""></Button>
                                <Button icon="pi pi-trash" type="button" class="p-button-danger p-mb-1" @click=""></Button> -->
                            </template>
                        </Column>
                    </DataTable>


                    <Toolbar class="p-mb-4">
                        <template v-slot:start>

                        </template>

                        <template v-slot:end>
                            <label for="subtotal" class="mr-2">SUBTOTAL: </label>
                            <InputNumber v-model="total.subtotalProducto" type="numeric" mode="currency" currency="USD" locale="en-US" placeholder="$" class="mr-2" readonly/>

                            <label for="iva" class="mr-2">IVA (16%): </label>
                            <InputNumber v-model="total.ivaProducto" type="numeric" mode="currency" currency="USD" locale="en-US" placeholder="$" class="mr-2" readonly/>

                            <label for="total" class="mr-2">TOTAL: </label>
                            <InputNumber v-model="total.totalProducto" type="numeric" mode="currency" currency="USD" locale="en-US" placeholder="$" readonly/>
                        </template>
                    </Toolbar>

                </Panel>

                <Dialog v-model:visible="editar.visibleEditar" modal header="DETALLES DE PRODUCTO" :style="{ width: '30vw' }" :draggable="false">

                    <div class="field">
                        <label for="nombre" class="mr-2">NOMBRE: </label>
                        <InputText id="nombre" type="text" size="40" placeholder="NOMBRE DEL PRODUCTO..." v-model="editar.tablaEditar.nombreE" class="p-inputtext-sm mr-2"/>
                    </div>

                    <div class="field">
                        <label for="cantidad" class="mr-2">CANTIDAD: </label>
                        <InputNumber  id="cantidad" type="numeric" placeholder="CANTIDAD" v-model="editar.tablaEditar.cantidadE" :min="1" class="p-inputtext-sm mr-2"/>
                    </div>

                    <div class="field">
                        <label for="precio" class="mr-2">PRECIO: </label>
                        <InputNumber  id="precio" type="numeric" placeholder="$ PRECIO" v-model="editar.tablaEditar.precioE"  mode="currency" currency="USD" locale="en-US" class="p-inputtext-sm mr-2"/>
                    </div>

                    <template #footer>
                        <Button label="GUARDAR" icon="pi pi-save" severity="success" @click="pasarDatosEditados" style="font-size: 1rem" autofocus />
                        <!-- :disabled="disabledBtnE" -->
                    </template>
                </Dialog>

                <ConfirmDialog></ConfirmDialog>
                <Toast/>
            </div>
        </div>
    </div>

</template>

<script>

    export default {
        data() {
            return {
                tablaCompras:[],
                // tablaCompras: [
                    /*{"cns": 1, "nomProducto": "Impresora LaserJet Color", "cantidad": 2, "precioUnitario": 5200.00, "precioParcial": 10400.00},
                    {"cns": 2, "nomProducto": "Monitor LED 31 plg.", "cantidad": 3, "precioUnitario": 1700.00, "precioParcial": 5100.00}*/
                // ],

                productoItem: {
                    id: 1,
                    nombreProducto: null,
                    cantidadProducto: null,
                    precioUnitario: null,
                },

                total: {
                    subtotalProducto: null,
                    ivaProducto: null,
                    totalProducto: null,
                },

                editar: {
                    visibleEditar: null,
                    tablaEditar: {
                        idE: null,
                        nombreE: null,
                        cantidadE: null,
                        precioE: null,
                    },

                }


            }
        },
        created() {

        },

        computed: {

            disabledBtnR() {
                if (this.productoItem.nombreProducto && this.productoItem.cantidadProducto && this.productoItem.precioUnitario) {
                    return false;
                } else {
                    return true;
                }
            },

            disabledBtnE() {
                for (var i = 0; i < this.tablaCompras.length; i++) {
                    if (this.tablaCompras[i].idP === this.editar.tablaEditar.idE) {
                        if (this.tablaCompras[i].nombreP != this.editar.tablaEditar.nombreE && this.tablaCompras[i].cantidadP != this.editar.tablaEditar.cantidadE && this.tablaCompras[i].precioP != this.editar.tablaEditar.precioE) {
                            return false;
                        } else {
                            return true;
                        }
                    }
                }
            },

        },

        methods: {

            formatoMoneda(value) {
                if(value)
                    return value.toLocaleString('en-US', {style: 'currency', currency: 'USD'});
                return;
            },

            registrarCompra() {

                const producto = {
                    idP: this.productoItem.id++,
                    nombreP: this.productoItem.nombreProducto.toUpperCase(),
                    cantidadP: this.productoItem.cantidadProducto,
                    precioP: this.productoItem.precioUnitario,
                    importeP: this.productoItem.cantidadProducto * this.productoItem.precioUnitario
                };

                //this.productoItem.id = this.productoItem.cns;
                //this.tablaCompras.length ;
                //this.productoItem.precioImporte = this.productoItem.precioUnitario * this.productoItem.cantidadProducto;
                // JSON.parse(this.productoItem);
                this.tablaCompras.push(producto);
                //this.productoItem.cns++;

                //this.tablaCompras.push(this.productoItem);
                this.$toast.add({severity:'success', summary: 'CONFIRMACION', detail: 'PRODUCTO AGREGADO CORRECTAMENTE', life: 2000});
                this.hacerTotal();
                this.limpiarEntrada();
            },

            limpiarEntrada() {
                this.productoItem.nombreProducto = null;
                this.productoItem.cantidadProducto = null;
                this.productoItem.precioUnitario = null;
                this.productoItem.precioImporte = null;
            },

            hacerTotal() {

                if (this.tablaCompras.length != 0) {
                    this.total.subtotalProducto = null;

                    for (var i = 0; i < this.tablaCompras.length; i++) {
                        this.total.subtotalProducto += this.tablaCompras[i].importeP;
                        this.total.ivaProducto = this.total.subtotalProducto * (0.16);
                        this.total.totalProducto = this.total.subtotalProducto + this.total.ivaProducto;
                    }
                } else {
                    this.total.subtotalProducto = null;
                    this.total.ivaProducto = null;
                    this.total.totalProducto = null;

                }
                // this.total.subtotalProducto += this.productoItem.cantidadProducto * this.productoItem.precioUnitario
                // this.total.ivaProducto = this.total.subtotalProducto * (0.16);
                // this.total.totalProducto = this.total.subtotalProducto + this.total.ivaProducto;
            },

            editarProductos(producto) {
                this.editar.tablaEditar.idE = producto.idP;
                this.editar.tablaEditar.nombreE = producto.nombreP;
                this.editar.tablaEditar.cantidadE = producto.cantidadP;
                this.editar.tablaEditar.precioE = producto.precioP;
                this.editar.visibleEditar = true;
            },

            pasarDatosEditados() {
                for (var i = 0; i < this.tablaCompras.length; i++) {
                    if (this.tablaCompras[i].idP === this.editar.tablaEditar.idE) {
                        this.tablaCompras[i].nombreP = this.editar.tablaEditar.nombreE;
                        this.tablaCompras[i].cantidadP = this.editar.tablaEditar.cantidadE;
                        this.tablaCompras[i].precioP = this.editar.tablaEditar.precioE;
                        this.tablaCompras[i].importeP = this.editar.tablaEditar.cantidadE * this.editar.tablaEditar.precioE;

                    }
                }
                this.$toast.add({ severity: 'info', summary: 'CONFIRMACION', detail: 'DATOS EDITATOS CORRECTAMENTE', life: 1900 });
                this.hacerTotal();
            },

            confirmaEliminarProductos(producto) {
                this.$confirm.require({
                    message: 'DESEAS ELIMINAR ESTE PRODUCTO?',
                    header: 'CONFIRMACION DE ELIMINACION',
                    icon: 'pi pi-info-circle',
                    acceptClass: 'p-button-danger',
                    accept: () => {
                        const index = this.tablaCompras.indexOf(producto);
                        this.tablaCompras.splice(index, 1);
                        this.hacerTotal();
                        this.$toast.add({ severity: 'info', summary: 'CONFIRMACION', detail: 'PODUCTO ELIMINADO', life: 3000 });
                    },
                    reject: () => {
                        this.$toast.add({ severity: 'error', summary: 'RECHAZO', detail: 'NO SE HA ELIMINADO EL PRODUCTO', life: 3000 });
                    }
                });
            },
        }
    }
</script>
