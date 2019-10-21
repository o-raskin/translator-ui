<template>
    <v-container fluid align="center" justify="center" ml-0 mr-0>
        <v-layout text-center wrap>
            <v-flex>
                <v-row>
                    <Fiddle :sides.sync="sides" :updateInterval.sync="speed" :minRadius.sync="radius"/>
                </v-row>
                <v-row align="center" justify="center">
                    <v-alert v-if="inputText.length === 0 && resultText.length === 0" dense type="info" min-width="10%"
                             max-width="60%">
                        {{phrase[0]}}
                    </v-alert>
                    <v-alert v-else-if="resultText.length !== 0" dense type="success" min-width="30%" max-width="50%">
                        {{phrase[1]}}
                    </v-alert>
                    <v-alert v-else-if="inputText.length !== 0 && resultText.length === 0" dense type="info" min-width="10%"
                             max-width="40%">
                        {{phrase[2]}}
                    </v-alert>
                </v-row>
                <v-row justify="center">
                    <v-col md="5">
                        <v-textarea id="source" background-color="white" outlined auto-grow
                                    :label="source.title" counter="true" v-model="inputText"
                                    :value="source.tag" v-on:input="translate" clearable @click:clear="reset" @input="computeWords">
                        </v-textarea>
                    </v-col>
                    <v-col md="1">
                        <v-btn class="mt-11" v-on:click="swap" text icon color="primary">
                            <v-icon>mdi-swap-horizontal</v-icon>
                        </v-btn>
                    </v-col>
                    <v-col md="5">
                        <v-textarea id="result" background-color="white" outlined auto-grow :label="target.title"
                                    contenteditable="false" hide-details v-model="resultText" :value="target.tag" disabled>
                        </v-textarea>
                    </v-col>
                </v-row>
            </v-flex>
        </v-layout>
    </v-container>
</template>


<script>
    import axios from 'axios';
    import _ from 'lodash';
    import Fiddle from '../fiddle/Fiddle';
    import {HOST} from "../../properties";
    import {PORT} from "../../properties";
    import {PROTOCOL} from "../../properties";


    export default {
        name: 'Core',
        components: {
            Fiddle
        },
        data: () => ({
            sides: 0,
            speed: 0,
            radius: 15,
            inputText: '',
            resultText: '',
            source: {
                title: 'Russian',
                tag: 'ru'
            },
            target: {
                title: 'Italian',
                tag: 'it'
            },
            phrase: [
                'Just write something...',
                'We have some translation for you!',
                'Hmmm...'
            ]
        }),
        methods: {
            computeWords() {
                this.sides = _.words(this.inputText).length
                this.speed = 450
                this.radius = 15
                this.translate;
            },
            translate: _.debounce(function () {
                axios.post(
                    `${PROTOCOL}://${HOST}:${PORT}/api/translate`,
                    {
                        source_lang: this.source.tag,
                        target_lang: this.target.tag,
                        text: this.inputText
                    })
                    .then((value) => {
                        this.resultText = value.data.translation
                        this.speed = 950
                        this.radius = 65
                    })
            }, 650),
            swap() {
                if (this.source.tag === 'ru') {
                    this.source.tag = 'it'
                    this.source.title = 'Italian'

                    this.target.tag = 'ru'
                    this.target.title = 'Russian'
                } else {
                    this.source.tag = 'ru'
                    this.source.title = 'Russian'
                    this.target.tag = 'it'
                    this.target.title = 'Italian'
                }
                this.inputText = this.resultText
                this.computeWords()
                this.translate()
            },
            reset() {
                this.inputText = ''
                this.resultText = ''
                this.sides = 0
            }
        }
    }
</script>