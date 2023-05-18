<template>
    <div class="grid">
        <div class="col-12 md:col-6">
            <div class="card p-fluid">
                <h5>GENERAR CODIGO A PRODUCTO</h5>
                    <div class="field">
                        <label for="cod">CODIGO DEL PRODUCTO</label>
                        <InputText id="cod" type="text" v-model="codigoProducto"/>
                    </div>
                    <div class="field">
                        <label for="nom">NOMBRE</label>
                        <InputText id="nom" type="text" v-model="nombreProducto" />
                    </div>
                    <div class="field">
                        <label for="prec">PRECIO</label>
                        <InputNumber id="pre" v-model="precioProducto" type="numeric" mode="currency" currency="USD" locale="en-US"/>
                    </div>
                    <div class="field">
                        <Button type="button" label="GENERAR QR" severity="help" :disabled="disabledBtn" @click="generarQR()"></Button>
                    </div>
            </div>
        </div>

        <div  v-if="verQR" class="col-12 md:col-6">
            <div class="card p-fluid">
                <div class="field col" style="text-align:center">
                    <qrcode-vue :value="qr" size="250" level="H" />
                </div>
            </div>
        </div>

        <Toast position="bottom-right" group="br"/>
    </div>

</template>

<script>
    import QrcodeVue from 'qrcode.vue'

    export default {
        data() {
            return {
                qr: null,
                codigoProducto: null,
                nombreProducto: null,
                precioProducto: null,
                verQR: false,

            }
        },

        components: {
          QrcodeVue,
        },

        computed: {
            disabledBtn() {
                if (this.codigoProducto && this.nombreProducto && this.precioProducto) {
                    return false;
                } else {
                    return true;
                    this.verQR = false;
                }
            },
        },

        methods: {
            generarQR() {
                this.qr = "CODIGO: " + this.codigoProducto.toUpperCase() + "\n";
                this.qr += "NOMBRE PRODUCTO: " + this.nombreProducto.toUpperCase() + "\n";
                this.qr += "PRECIO: " + this.codigoProducto + "\n";
                this.$toast.add({severity:'success', summary: 'CONFIRMACION', detail: 'CODIGO GENERADO CORRECTAMENTE', group: 'br', life: 2000});
                this.verQR = true;

            }

        },
    }

</script>
