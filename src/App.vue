<script setup>
import { ref, computed } from 'vue'

const memoria_inicio = ref(10 + 50)
const kernel = ref(10 + 9)
const acumulador = ref(0)
const valor = ref();

const instrucciones = ref(new Array(10).fill("none"))
const memoriaPrincipal = ref(new Array(memoria_inicio.value));
const variables = ref(new Array(10).fill("none"));
const etiquetas = ref(new Array(10).fill("none"))

const nombresValores = ref({});
const mostratModal = ref(false);

const diccionario = ["nueva", "cargue", "almacene", "lea", "sume", "reste", "multiplique", "divida", "potencia",
    "modulo", "concatene", "elimine", "extraiga", "Y", "O", "NO", "muestre", "imprima", "retorne", "vaya", "vayasi", "etiqueta"]

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

                    if (diccionario.includes(element.split(" ")[0].toLowerCase())) {
                        element.trim()
                        const expRegular = /(\s{2,})/g;
                        let reemplazar = element.replace(expRegular, " ");
                        instrucciones.value.push(reemplazar)
                        if (reemplazar.split(" ")[0].toLowerCase() == "nueva") {
                            variables.value.push(reemplazar.split(" ")[1])
                        }
                        if (reemplazar.split(" ")[0].toLowerCase() == "variable") {
                            variables.value.push(reemplazar.split(" ")[1])
                        }
                        if (reemplazar.split(" ")[0].toLowerCase() == "etiqueta") {
                            etiquetas.value.push(reemplazar.split(" ")[1])
                        }
                    }
                });
                for (let i = 1; i <= kernel.value; i++) {
                    memoriaPrincipal.value[i] = "--** CH MAQUINA **--"
                }
                for (let i = 0; i < instrucciones.value.length; i++) {
                    memoriaPrincipal.value[i + kernel.value + 1] = instrucciones.value[i]
                }
            }
        };
        reader.readAsText(file);
    };
    input.click();
}

const ejecutar = () => {
    for (let i = 0; i < instrucciones.value.length; i++) {
        switch (instrucciones.value[i].split(" ")[0].toLowerCase()) {
            case "cargue":
                acumulador.value = nombresValores.value[instrucciones.value[i].split(" ")[1]] = 0;
                memoriaPrincipal.value[0] = acumulador.value;
                break;

            case "almacene":
                nombresValores.value[instrucciones.value[i].split(" ")[1]] = acumulador.value
                memoriaPrincipal.value[0] = acumulador.value;
                break;

            case "nueva":
                if (instrucciones.value[i].split(" ")[2].toLowerCase == "c") {
                    nombresValores.value[instrucciones.value[i].split(" ")[1]] = "" + instrucciones.value[i].split(" ")[3]
                }
                else if (instrucciones.value[i].split(" ")[2].toLowerCase == "i") {
                    nombresValores.value[instrucciones.value[i].split(" ")[1]] = Number(instrucciones.value[i].split(" ")[3])
                }
                else if (instrucciones.value[i].split(" ")[2].toLowerCase == "r") {
                    nombresValores.value[instrucciones.value[i].split(" ")[1]] = parseFloat(instrucciones.value[i].split(" ")[3])
                }
                else {
                    nombresValores.value[instrucciones.value[i].split(" ")[1]] = instrucciones.value[i].split(" ")[3]
                }
                break;

            case "lea":
                const input = prompt("Ingrese un valor" + element.split(" ")[1]);
                for (const key in nombresValores.value) {
                    if (key == element.split(" ")[1]) {
                        nombresValores.value[key] = input
                    }
                    else {
                        nombresValores.value[element.split(" ")[1]] = input
                    }
                }
                break;

            case "sume":
                acumulador.value += nombresValores.value[element.split(" ")[1]]
                break;

            case "reste":
                acumulador.value = acumulador.value - nombresValores.value[element.split(" ")[1]]
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "multiplique":
                acumulador.value = acumulador.value * nombresValores.value[element.split(" ")[1]]
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "divida":
                acumulador.value = acumulador.value / nombresValores.value[element.split(" ")[1]]
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "potencia":
                acumulador.value = acumulador.value ** nombresValores.value[element.split(" ")[1]]
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "modulo":
                acumulador.value = acumulador.value % nombresValores.value[element.split(" ")[1]]
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "concatene":
                acumulador.value = acumulador.value + nombresValores.value[element.split(" ")[1]]
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "elimine":
                acumulador.value = acumulador.value.slice(0, -1)
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "extraiga":
                acumulador.value = acumulador.value.slice(0, 1)
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "y":
                if (acumulador.value == true && nombresValores.value[element.split(" ")[1]] == true) {
                    acumulador.value = true
                }
                else {
                    acumulador.value = false
                }
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "o":
                if (acumulador.value == true || nombresValores.value[element.split(" ")[1]] == true) {
                    acumulador.value = true
                }
                else {
                    acumulador.value = false
                }
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "no":
                if (acumulador.value == true) {
                    acumulador.value = false
                }
                else {
                    acumulador.value = true
                }
                memoriaPrincipal.value[0] = acumulador.value
                break;

            case "muestre":
                alert(acumulador.value)
                break;

            case "imprima":
                alert(nombresValores.value[element.split(" ")[1]])
                break;
            case "retorne":
                alert("El programa ha finalizado")

            case "vaya":
                instrucciones.value.forEach((element2, index) => {
                    if (element2.split(" ")[0] == element.split(" ")[1]) {
                        instrucciones.value = instrucciones.value.slice(index)
                    }
                })
                break;
            case "vayasi":
                if (acumulador > 0) {
                    instrucciones.value.forEach((element2, index) => {
                        if (element2.split(" ")[0] == element.split(" ")[1]) {
                            instrucciones.value = instrucciones.value.slice(index)
                        }
                    })
                }

            case "etiqueta":
                /*instrucciones.value.forEach((element2, index) => {
                    if(element2.split(" ")[0] == element.split(" ")[1]){
                        instrucciones.value = instrucciones.value.slice(index)
                    }
                })*/
                for (let i = 0; i < instrucciones.value.length; i++) {
                    if (instrucciones.value[i].split(" ")[0] == element.split(" ")[1]) {
                        instrucciones.value = instrucciones.value.slice(i)
                    }
                }

        }
    }
}

</script>

<template>
    <div class="container text-center">
        <nav class="navbar navbar-expand-lg bg-dark mb-3">
            <div class="container-fluid">
                <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
                    <div class="navbar-nav">
                        <a class="nav-link" aria-current="page" href="#" @click="cargar">Abrir</a>
                        <a class="nav-link" href="#" @click="ejecutar">Ejecute</a>
                        <a class="nav-link">Pausa</a>
                        <a class="nav-link" href="#" @click="iniciarPaso">Paso a paso</a>
                        <a class="nav-link">Salir</a>
                    </div>
                </div>
            </div>
        </nav>
        <div class="row">
            <!--COLUMNA UNO-->
            <div class="col">
                <div class="row border border-dark p-3">
                    <div class="col">
                        <label for="kernel">Kernel
                            <input class="small" type="number" v-model="kernel" id="kernel">
                        </label>
                        <br>
                        <br>
                        <label for="m_principal" class="">Memoria p.
                            <input class="small" type="number" v-model="memoria_inicio" id="m_principal">
                        </label>
                    </div>
                    <div class="row"></div>
                </div>

                <div class="row py-3">
                    <!--SUBCOLUMNA UNO-->
                    <div class="col">
                        <table class="table table-striped table-hover border border-dark" id="tablapmas">
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

                    <!--SUBCOLUMNA DOS-->
                    <div class="col">
                        <table class="table table-striped table-hover border border-dark" id="tablapmas">
                            <!--table table-dark table-striped-columns-->
                            <thead>
                                <tr>
                                    <th scope="col">Pos</th>
                                    <th scope="col">Variables</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(cont,index) in variables" :key="index">
                                    <th scope="row">{{index}}</th>
                                    <td>{{cont}}</td>
                                </tr>
                            </tbody>
                        </table>
                        <table class="mt-3 table table-striped table-hover border border-dark" id="tablapmas">
                            <thead>
                                <tr>
                                    <th scope="col">Pos</th>
                                    <th scope="col">Etiquetas</th>
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

            <!--COLUMNA DOS-->
            <div class="col px-4">
                <div>
                    <img src="imagenes/screen.jpg" height="250" width="335" />
                </div>
                <br>
                <br>
                <div>
                    <img src="imagenes/impresora_rec.jpg" height="300" width="350" />
                </div>
            </div>

            <!--COLUMNA TRES-->
            <div class="col">
                <table class="table table-striped table-hover border border-dark">
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
    color: pink;
}

.navbar a:focus {
    color: lightblue;
}

/*.table-striped>tbody>tr:nth-child(odd)>td, 
.table-striped>tbody>tr:nth-child(odd)>th {
   background-color: #E5D1D0; }*/

.small {
    width: 20%;
}
</style>
