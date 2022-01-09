<template>
    <div>
        <v-app>
            <v-container>
                <v-row>
                    <v-col cols="12">
                        <v-btn @click="select(-1)">Piso -1</v-btn><br />
                        <v-btn @click="select(0)"> Piso 0</v-btn><br />
                        <v-btn @click="select(1)"> Piso 1</v-btn><br />
                        <v-btn @click="select(2)"> Piso 2</v-btn><br />
                        <v-btn @click="select(3)"> Piso 3</v-btn>
                    </v-col>
                    <v-col cols="12">
                        <v-btn @click="getSvg"> get svg</v-btn>
                        <object width="300px" id="t" :data="img"></object>
                    </v-col>

                    <v-col cols="12">
                        <map-analysis :svg="svg" />
                    </v-col>
                </v-row>
            </v-container>
        </v-app>
    </div>
</template>

<script>
import piso_1 from "./assets/piso-1.svg";
import piso1 from "./assets/piso1.svg";
import piso0 from "./assets/piso0.svg";
import piso2 from "./assets/piso2.svg";
import piso3 from "./assets/piso3.svg";
import mapAnalysis from "./components/MapAnalysis.vue";
export default {
    name: "App",
    data() {
        return {
            img: null,
            svg: null,
        };
    },
    methods: {
        select(i) {
            switch (i) {
                case 0:
                    this.img = piso0;
                    break;
                case 1:
                    this.img = piso1;
                    break;
                case 2:
                    this.img = piso2;
                    break;
                case 3:
                    this.img = piso3;
                    break;
                default:
                    this.img = piso_1;
                    break;
            }
        },
        getSvg() {
            this.svg = document
                .getElementById("t")
                .contentDocument.querySelector("svg").outerHTML;
        },
    },
    components: {
        mapAnalysis,
    },
};
</script>
