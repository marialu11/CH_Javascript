<script setup>
import { ref, computed } from 'vue'

const memoria_inicio = ref(10 + 50)
const kernel = ref(10 + 9)

const instrucciones = ref(new Array(10).fill("none"))
const memoriaPrincipal = ref(new Array(memoria_inicio.value));
const variables = ref(new Array(10).fill("none"));
const etiquetas = ref(new Array(10).fill("none"))

const cargar = () => {
    const input = document.createElement("input");
    input.type = "file";
    input.onchange = (e) => {
        const file = e.target.files[0];
        const reader = new FileReader();
        reader.onload = (e) => {
            const text = e.target.result;
            console.log(text)
            if (text) {
                const lines = text.toString().split("\n");
                //console.log(lines)
                instrucciones.value = []
                variables.value = []
                etiquetas.value = []
                lines.forEach(element => { 
                    element = element.trim();
                    if(element.includes("nueva")){
                        let a = element.split(" ")
                        if (a[1] == ""){
                            variables.value.push(a[2]);
                        }
                        else{
                            variables.value.push(a[1])
                        }
                    }
                    if(element.includes("etiqueta")){
                        let a = element.split(" ")
                        etiquetas.value.push(a[1]);
                    }
                    if(element.includes("//") == false && element != "")
                    {
                        console.log(element)
                        instrucciones.value.push(element)
                    }
                });
                for (let i = 1; i<= kernel.value; i++) {
                    memoriaPrincipal.value[i] = "--** CH MAQUINA **--"
                }
                for (let i = 0; i< instrucciones.value.length; i++){
                    memoriaPrincipal.value[i + kernel.value + 1] = instrucciones.value[i]
                }
            }
        };
        reader.readAsText(file);
    };
    input.click();
}

</script>

<template>
    <div class="container text-center">
        <nav class="navbar navbar-expand-lg bg-dark mb-3">
            <div class="container-fluid">
                <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
                    <div class="navbar-nav">
                        <a class="nav-link" aria-current="page" href="#" @click="cargar">Abrir</a>
                        <a class="nav-link">Ejecute</a>
                        <a class="nav-link">Pausa</a>
                        <a class="nav-link">Paso a paso</a>
                        <a class="nav-link">Salir</a>
                    </div>
                </div>
            </div>
        </nav>
        <div class="row">
            <div class="col">
                <h3>Kernel</h3>
                <input type="number" v-model="kernel">
                <h3>Memoria principal</h3>
                <input type="number" v-model="memoria_inicio">
                <div class="row py-3">
                    <div class="col">
                        <table class="table table-dark table-striped-columns" id="tablapmas">
                            <thead>
                                <tr>
                                    <th scope="col">Dirección</th>
                                    <th scope="col">Contenido</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(cont,index) in instrucciones" :key="index">
                                    <th scope="row">{{index}}</th>
                                    <td>{{cont}}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="col">
                        <table class="table table-dark table-striped-columns" id="tablapmas">
                            <thead>
                                <tr>
                                    <th scope="col">Dirección</th>
                                    <th scope="col">Contenido</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(cont,index) in variables" :key="index">
                                    <th scope="row">{{index}}</th>
                                    <td>{{cont}}</td>
                                </tr>
                            </tbody>
                        </table>
                        <table class=" mt-3 table table-dark table-striped-columns" id="tablapmas">
                            <thead>
                                <tr>
                                    <th scope="col">Dirección</th>
                                    <th scope="col">Contenido</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(cont,index) in etiquetas" :key="index">
                                    <th scope="row">{{index}}</th>
                                    <td>{{cont}}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>

                </div>

            </div>
            <div class="col">
                <h3>Col 3</h3>
            </div>
            <div class="col">
                <table class="table table-dark table-striped">
                    <thead>
                        <tr>
                            <th scope="col">Dirección</th>
                            <th scope="col">Contenido</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(cont,index) in memoriaPrincipal" :key="index">
                            <th scope="row">{{index}}</th>
                            <td>{{cont}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>

    </div>
</template>
    
<style scoped>
.table {
    font-size: 13px;
}

#tablapmas {
    font-size: 13px;
}

#tablaVbles {
    font-size: 13px;
}

.nav-link {
    color: white;
}

.navbar a:hover {
    color:pink;
}
</style>
