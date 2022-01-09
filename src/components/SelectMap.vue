<template>
    <div>
        <v-stepper-step :complete="e6 > 1" :editable="e6 > 1" step="1">
            {{ "Upload Map" | translate }}
        </v-stepper-step>

        <v-stepper-content step="1">
            <v-card :elevation="15">
                <v-card-subtitle>
                    Recommendations:
                    <ul>
                        <li>
                            {{
                                "Delete the maps that you no longer use"
                                    | translate
                            }}
                        </li>
                        <li>
                            {{
                                "Do not load maps that weigh more than 5 mb"
                                    | translate
                            }}
                        </li>
                        <li>
                            {{
                                "Do not have more than three maps at the same time"
                                    | translate
                            }}
                        </li>
                    </ul>
                </v-card-subtitle>
                <v-card-text>
                    <h2 class="text-center mb-3">
                        {{ "Select a map" | translate }}
                    </h2>
                    <v-row>
                        <v-col cols="12">
                            <v-btn @click="getSvg"> get svg</v-btn>
                        </v-col>
                        <v-col cols="12">
                            <object width="300px" id="t" :data="img"></object>
                        </v-col>
                    </v-row>
                </v-card-text>
                <v-card-actions class="d-flex justify-center">
                    <v-btn color="primary" @click="$emit('changeStep')">
                        {{ "Continue" | translate }}
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-stepper-content>
    </div>
</template>

<script>
import mapa from "../assets/mapa1.svg";
export default {
    props: ["e6", "maps"],
    filters: {
        translate(val) {
            return val;
        },
    },
    data: () => ({
        img: mapa,

        svg: "",
    }),

    methods: {
        getSvg() {
            this.svg = document
                .getElementById("t")
                .contentDocument.querySelector("svg").outerHTML;
        },
    },
};
</script>