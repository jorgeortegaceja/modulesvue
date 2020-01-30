<template>
    <v-row justify="center" align="center">
        <v-col class="title text-center" cols="12">
            <strong>{{ title }}</strong>
        </v-col>
        <v-col class="text-white text-center" cols="12">
            {{ subtitle }}
        </v-col>

        <v-col v-for="(i, index) in items" :key="index" cols="11" sm="5" md="4" lg="3" xl="2" class="mt-2  center-flex">
            <p class="contenedor-de-botones">

                <button :disabled="disabled(i[itemValue])" v-ripple :class="{ active: active(i[itemValue]) }"
                    class="btn height overflow-hidden " :id="'p' + ' - ' + index"
                    :style="{ height: height, width: width }" @click="selectedItem(i[itemValue])">
                    <span class="vue-input-combo ">
                        <v-row justify="center" align="center">
                            <v-img :src="i.img" max-width="80" max-height="80" />
                        </v-row>
                    </span>
                    <p class="overflow-hidden">
                        {{ i[itemText] }}
                    </p>
                </button>
            </p>
        </v-col>

        <v-col cols="12" class="error--text text-center">
            {{ errorMessage }}


        </v-col>


    </v-row>
</template>
<script>
    export default {
        props: {
            value: {
                type: [Number, Array, String]
            },
            items: {
                type: Array,
                default: () => []
            },
            itemValue: {
                type: [String, Array, Function],
                default: "value"
            },
            itemText: {
                type: [String, Array, Function],
                default: "text"
            },
            title: {
                type: [String],
                default: "title"
            },
            subtitle: {
                type: [String],
                default: "subtitle"
            },
            max: {
                type: [Number, String],
                default: null
            },
            nothing: {
                type: [Number, String],
                default: null
            },
            multiple: {
                type: [String, Boolean]
            },
            errorMessage: {
                type: [String]
            },
            height: {
                type: [String, Number],
                default: "100%"
            },
            width: {
                type: [String, Number],
                default: "100%"
            }
        },
        beforeMount() {
            this.mp =
                this.multiple === false || this.multiple == "false" ? false : true;
            this.selectedItems = this.value != "" ? this.value : [];
        },

        data() {
            return {
                selectedItems: [],
                mp: true,
            };
        },


        methods: {
            disabled(i) {
                if (this.mp) {
                    if (this.max != null) {
                        if (this.selectedItems.length < this.max) {
                            return false;
                        } else {
                            if (this.selectedItems.includes(i)) {
                                return false;
                            } else {
                                return true;
                            }
                        }
                    }
                }
            },
            active(i) {
                if (this.mp) {
                    return this.selectedItems.includes(i);
                } else {
                    return this.selectedItems == i;
                }
            },
            selectedItem(i) {
                if (this.mp) {
                    if (this.nothing != i) {
                        if (this.selectedItems.includes(this.nothing * 1)) {
                            this.selectedItems.splice(0, 1);
                        }
                        if (this.selectedItems.includes(i)) {
                            this.selectedItems.splice(this.selectedItems.indexOf(i), 1);
                        } else {
                            this.selectedItems.push(i);
                        }
                    } else {
                        this.selectedItems = [];
                        let a = this.nothing * 1;
                        this.selectedItems.push(a);
                    }

                    this.$emit("input", this.selectedItems);
                } else {
                    this.selectedItems = i;
                    this.$emit("input", i);
                }
            }
        }
    };
</script>
