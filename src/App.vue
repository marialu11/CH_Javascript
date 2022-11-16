<script setup>

import { ref, computed } from 'vue'

const memoria_inicio = ref(10 + 50)
const kernel = ref(10 + 9)
const acumulador = ref(0);
/*const valor = ref();*/
const programas = ref({});
const identificadorPrograma = ref(0);
const imprimirTexto = ref("")
const imprimirTextoi = ref("")

const instrucciones = ref([])
const memoriaPrincipal = ref(new Array(memoria_inicio.value));
const variables = ref([]);
const etiquetas = ref([]);
const posicion = ref(0);

const nombresValores = ref({});

const diccionario = ["nueva", "cargue", "almacene", "lea", "sume", "reste", "multiplique", "divida", "potencia",
    "modulo", "concatene", "elimine", "extraiga", "Y", "O", "NO", "muestre", "imprima", "retorne", "vaya", "vayasi", "etiqueta"]

const cargar = () => {
    const input = document.createElement("input");
    input.type = "file";
    input.onchange = (e) => {
        const file = e.target?.files[0];
        const reader = new FileReader();
        reader.onload = (e) => {
            const text = e.target.result;
            console.log(text)
            if (text) {
                const lines = text.toString().split("\n");
                lines.forEach(element => {
                    element = element.trim();

                    if (diccionario.includes(element.split(" ")[0].toLowerCase())) {
                        const expRegular = /(\s{2,})/g;
                        let reemplazar = element.replace(expRegular, " ");
                        instrucciones.value.push(reemplazar)
                        if (reemplazar.split(" ")[0].toLowerCase() == "nueva") {
                            variables.value.push(reemplazar.split(" ")[1])
                        }
                        else if (reemplazar.split(" ")[0].toLowerCase() == "etiqueta") {
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
                programas.value[identificadorPrograma.value] = instrucciones.value
                identificadorPrograma.value += 1
                console.log(programas.value)
            }
        };
        reader.readAsText(file);
    };
    input.click();
}

const ejecutar = (i) => {
    console.log(instrucciones.value[i].toLowerCase())
    switch (instrucciones.value[i].split(" ")[0].toLowerCase()) {
        case "cargue":
            acumulador.value = nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "almacene":
            nombresValores.value[instrucciones.value[i].split(" ")[1]] = acumulador.value
            break;

        case "nueva":
        
            if (instrucciones.value[i].split(" ")[2].toLowerCase == "c") {
                nombresValores.value[instrucciones.value[i].split(" ")[1]] = "" + instrucciones.value[i].split(" ")[3]
            }
            else if (instrucciones.value[i].split(" ")[2].toLowerCase == "i") {
                console.log("entro")
                nombresValores.value[instrucciones.value[i].split(" ")[1]] = Number(instrucciones.value[i].split(" ")[3])
            }
            else if (instrucciones.value[i].split(" ")[2].toLowerCase == "r") {
                nombresValores.value[instrucciones.value[i].split(" ")[1]] = parseFloat(instrucciones.value[i].split(" ")[3])
            }
            else {
                nombresValores.value[instrucciones.value[i].split(" ")[1]] = instrucciones.value[i].split(" ")[3] == 1 ? true : false
            }
            break;

        case "lea":
            nombresValores.value[instrucciones.value[i].split(" ")[1]] = prompt("Ingrese un valor")
            break;

        case "sume":
            acumulador.value += nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "reste":
            acumulador.value -= nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "multiplique":
            acumulador.value *= nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "divida":
            acumulador.value /= nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "potencia":
            acumulador.value = Math.pow(acumulador.value, nombresValores.value[instrucciones.value[i].split(" ")[1]])
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "modulo":
            acumulador.value %= nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "concatene":
            acumulador.value += nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value;
            break;

        case "elimine":
            acumulador.value = acumulador.value.slice(0, -1)
            memoriaPrincipal.value[0] = acumulador.value
            break;

        case "extraiga":
            acumulador.value = acumulador.value.slice(0, nombresValores.value[instrucciones.value[i].split(" ")[1]])
            memoriaPrincipal.value[0] = acumulador.value
            break;

        case "y":
            acumulador.value = acumulador.value && nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value
            break;

        case "o":
            acumulador.value = acumulador.value || nombresValores.value[instrucciones.value[i].split(" ")[1]]
            memoriaPrincipal.value[0] = acumulador.value
            break;

        case "no":
            acumulador.value = !acumulador.value
            memoriaPrincipal.value[0] = acumulador.value
            break;

        case "muestre":
            if (instrucciones.value[i].split(" ")[1] == "acumulador") {
                imprimirTexto.value += acumulador.value + "\n"
                console.log(imprimirTexto.value)
            }
            else {
                imprimirTexto.value += nombresValores.value[instrucciones.value[i].split(" ")[1]] + "\n"
                console.log(imprimirTexto.value)
            }
            break;

        case "imprima":
            if (instrucciones.value[i].split(" ")[1] == "acumulador") {
                imprimirTextoi.value += acumulador.value + "\n"
            }
            else {
                imprimirTextoi.value += nombresValores.value[instrucciones.value[i].split(" ")[1]] + "\n"
            }
            break;

        case "retorne":
            alert("El programa ha finalizado")

            break;

        case "vaya":
            i = instrucciones.value.findIndex(instrucciones.value[i].split(" ")[1])
            break;
        case "vayasi":
            if (acumulador.value > 0) {
                const a = instrucciones.value.find(
                    function (element) {
                        if (element.split(" ")[0] == "etiqueta" && element.split(" ")[1] === instrucciones.value[i].split(" ")[1]) {
                            return element.split(" ")[2]
                        }
                    }
                )
                return i = parseInt(a.split(" ")[2]) - 2
            }

            else if (acumulador.value < 0) {
                const a = instrucciones.value.find(
                    function (element) {
                        if (element.split(" ")[0] === "etiqueta" && element.split(" ")[1] === instrucciones.value[i].split(" ")[2]) {
                            return element.split(" ")[2]
                        }
                    }
                )
                return i = parseInt(a.split(" ")[2]) - 2
            }
            else
                break;
    }
}

const ejecutarTodo = () => {
    for (let i = 0; i < instrucciones.value.length; i++) {
        console.log(i)
        if (ejecutar(i)) {
            i = ejecutar(i)
        }
    }
}

function pasoPaso() {
    if (posicion.value < instrucciones.value.length) {
        console.log(posicion.value)
        
        if (ejecutar(posicion.value))
            posicion.value = ejecutar(posicion.value);
        posicion.value++
    }
    else {
        alert("Se ha terminado de ejecutar el programa")
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
                        <a class="nav-link" href="#" @click="ejecutarTodo">Ejecute</a>
                        <a class="nav-link" href="#" @click="pasoPaso">Paso a paso</a>
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
                                <tr v-for="(cont, index) in instrucciones" :key="index">
                                    <th scope="row">{{ index }}</th>
                                    <td>{{ cont }}</td>
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
                                <tr v-for="(cont, index) in variables" :key="index">
                                    <th scope="row">{{ index }}</th>
                                    <td>{{ cont }}</td>
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
                                <tr v-for="(cont, index) in etiquetas" :key="index">
                                    <th scope="row">{{ index }}</th>
                                    <td>{{ cont }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>

                </div>
            </div>

            <!--COLUMNA DOS-->
            <div class="col px-4">
                <div class="pantalla">
                    <img src="imagenes/screen.jpg" height="250" width="335" />
                    <textarea class="t_pantalla" v-model="imprimirTexto"></textarea>
                </div>
                <br>
                <br>
                <div class="impresora">
                    <img src="imagenes/impresora_rec.jpg" height="300" width="350" />
                    <textarea class="t_impresora" v-model="imprimirTextoi"></textarea>
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
                        <tr v-for="(cont, index) in memoriaPrincipal" :key="index">
                            <th scope="row">{{ index }}</th>
                            <td>{{ cont }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>
    
<style scoped>
.pantalla {
    position: relative;
    display: inline-block;
}

.t_pantalla {
    position: absolute;
    top: 10px;
    left: 5%;
    font-family: verdana;
    font-size: 22px;
    color: black;
    width: 250px;
    height: 100px;
    border: none;
}

.impresora {
    position: relative;
    display: inline-block;
}

.t_impresora {
    position: absolute;
    top: 9px;
    left: 27%;
    font-family: verdana;
    font-size: 22px;
    color: black;
    width: 158px;
    height: 97px;
    border: none;
}

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
