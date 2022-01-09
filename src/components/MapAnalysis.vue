<template>
    <div>
        <v-card>
            <v-card-text>
                <v-row>
                    <v-col cols="3" class="pt-15">
                        <br /><br /><br />
                        <v-btn @click="interactiveSpaces">
                            interactiveSpaces
                        </v-btn>
                        <br />
                        <br />
                        <v-text-field type="text" v-model="name" />
                        <v-btn @click="saveSvg"> save svg </v-btn>
                        <br />

                        <pre>{{ $data }}</pre>
                    </v-col>
                    <v-col cols="8">
                        <div v-html="svg"></div>
                    </v-col>
                </v-row>
            </v-card-text>
        </v-card>

        <v-dialog v-model="dialog" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="text-h5">Set Space</span>
                </v-card-title>
                <v-card-text>
                    <v-row>
                        <v-col cols="12">
                            <v-text-field
                                label="name"
                                v-model="space.name"
                            ></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-select
                                :items="items"
                                v-model="space.location_type"
                                label="location type"
                            ></v-select>

                            <v-text-field
                                v-model="space.location_type"
                                label="location type"
                            ></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-checkbox
                                v-model="space.is_reservable"
                                label="is Reservable"
                            ></v-checkbox>
                        </v-col>
                        <v-col cols="12">
                            <v-checkbox
                                v-model="space.active"
                                label="active"
                            ></v-checkbox>
                        </v-col>
                    </v-row>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false">
                        Close
                    </v-btn>
                    <v-btn
                        color="blue darken-1"
                        text
                        @click="saveInformation()"
                    >
                        Save
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>

<script>
import jQuery from "jquery";
import _ from "lodash";
export default {
    props: ["svg"],
    data: () => ({
        items: [
            "Escritorios",
            "Workspace/Desk",
            "Meeting room",
            "Sala de Juntas",
            "Oficina",
            "Cabinas Teléfonicas",
            "Sala de Juntas Dirección",
            "Sala de Lactancia",
            "Fisioterapia",
        ],
        item: "",
        count: 0,
        dialog: false,
        name: "",
        space: {
            name: "",
            location_type: "",
            is_reservable: "",
            active: "",
        },
    }),
    methods: {
        saveSvg() {
            let a = jQuery("svg>g[cursor]");
            for (var u = 0; u < a.length; u++) {
                let b = jQuery(a[u]);
                b.removeAttr("cursor");
                b.removeAttr("class");
                b.removeAttr("id");
            }

            this.$nextTick(() => {
                let filename = this.name;
                let text = jQuery("svg")[0].outerHTML;

                let element = document.createElement("a");
                element.setAttribute(
                    "href",
                    "data:image/svg+xml;charset=utf-8," +
                        encodeURIComponent(text)
                );
                element.setAttribute("download", filename);

                element.style.display = "none";
                document.body.appendChild(element);

                element.click();

                document.body.removeChild(element);
            });
        },
        printSpacellenos() {
            let a = jQuery("svg>g[name]");

            console.log(a);
            for (var u = 0; u < a.length; u++) {
                let b = jQuery(a[u]);
                b.addClass("space-selected");
            }
        },
        saveInformation() {
            for (const key in this.space) {
                let k = key.replace("_", "-");
                jQuery(`#${this.item}`).attr(k, this.space[key]);
                jQuery(`#${this.item}`).addClass("space-selected");
            }

            jQuery(`#${this.item}`).attr("interactive", "true");

            this.space = {
                name: "",
                location_type: "",
                is_reservable: "",
                active: "",
            };
            this.count++;
            this.dialog = false;
        },
        interactiveSpaces() {
            let a = document.querySelectorAll("svg>g");
            var self = this;
            for (var u = 0; u < a.length; u++) {
                let b = jQuery(a[u]);

                b.attr("id", _.uniqueId("space_"));
                b.attr("cursor", "pointer");

                b.hover(
                    function () {
                        jQuery(this).addClass("space-selected-hover");
                    },
                    function () {
                        jQuery(this).removeClass("space-selected-hover");
                    }
                );
                b.on("click", function () {
                    self.item = jQuery(this).attr("id");
                    self.space.location_type =
                        jQuery(this).attr("location-type") ?? "";
                    self.space.name = jQuery(this).attr("name") ?? "";
                    self.space.active = jQuery(this).attr("active") ?? true;
                    self.space.is_reservable =
                        jQuery(this).attr("is-reservable") ?? true;

                    self.dialog = true;
                });
            }
        },
    },
};
</script>
<style lang="scss">
.v-btn--icon {
    border: none !important;
}

.v-input__append-outer {
    margin: 0px !important;
}

.space-selected-hover,
.space-selected-hover > g > rect,
.space-selected-hover > g > path,
.space-selected-hover > path,
.space-selected-hover > rect,
.space-selected > polygon,
.space-selected > line,
.space-selected > ellipse,
.space-selected > circle {
    fill: #2d9437;
}

.space-selected,
.space-selected > g > rect,
.space-selected > g > path,
.space-selected > path,
.space-selected > rect,
.space-selected > polygon,
.space-selected > line,
.space-selected > ellipse,
.space-selected > circle {
    fill: #2d9437;
}

.space-selected-hover,
.space-selected-hover > g > rect,
.space-selected-hover > g > path,
.space-selected-hover > path,
.space-selected-hover > rect,
.space-selected > polygon,
.space-selected > line,
.space-selected > ellipse,
.space-selected > circle {
    fill: #d34f4c;
}

.buttons {
    position: absolute;
    width: 29px;
    height: 84px;
    z-index: 100;
    top: 80px;
    left: 15px;
}

/* 
    status:

    1: success
    2: warning
    3: error


*/
</style>