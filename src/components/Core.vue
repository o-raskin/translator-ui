<template>
    <v-container>
        <v-layout text-center wrap>
            <v-flex id="message-alert" mt-5>
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
            </v-flex>
            <v-container fluid>
                <v-row>
                    <v-col>
                        <v-btn v-on:click="swap" text icon color="primary">
                            <v-icon>mdi-swap-horizontal</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col md="6">
                        <v-textarea id="source" background-color="white" outlined auto-grow
                                    :label="source.title" counter="true" v-model="inputText" v-on:input="translate"
                                    :value="source.tag">
                        </v-textarea>
                    </v-col>
                    <v-col md="6">
                        <v-textarea id="result" background-color="white" outlined auto-grow :label="target.title"
                                    contenteditable="false" v-model="resultText" :value="target.tag" disabled>
                        </v-textarea>
                    </v-col>
                </v-row>
            </v-container>
            <!--            <Fiddle/>-->
        </v-layout>
    </v-container>
</template>


<script>
    import axios from 'axios';
    import _ from 'lodash';

    export default {
        name: 'Core',
        components: {
            // Fiddle
        },
        data: () => ({
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
            translate: _.debounce(function () {
                axios.post(
                    `http://localhost:8081/api/translate`,
                    {
                        source_lang: this.source.tag,
                        target_lang: this.target.tag,
                        text: this.inputText
                    })
                    .then((value) => {
                        this.resultText = value.data.translation
                    })
            }, 650),
            swap () {
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
                this.translate()
            }
        }
    }

</script>