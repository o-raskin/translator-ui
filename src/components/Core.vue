<template>
    <v-container>
        <v-layout text-center wrap>
            <v-flex id="message-alert" mt-5>
                <v-row align="center" justify="center">
                    <v-alert v-if="inputText === '' && resultText === ''" dense type="info" min-width="10%"
                             max-width="60%">
                        Just write something...
                    </v-alert>
                    <v-alert v-else-if="resultText !== ''" dense type="success" min-width="30%" max-width="50%">
                        We have some translation for you!
                    </v-alert>
                    <v-alert v-else-if="inputText !== '' && resultText === ''" dense type="info" min-width="10%"
                             max-width="40%">
                        Hmmm...
                    </v-alert>
                </v-row>
            </v-flex>
            <v-container fluid>
                <v-row>
                    <v-col class="mb-0 pb-0" cols="12" md="6">
                        <v-textarea id="source" background-color="white" outlined name="input-7-4" auto-grow
                                    label="Russian" counter="true" v-model="inputText" clearable v-on:input="translate"

                        ></v-textarea>
                    </v-col>
                    <v-col cols="12" md="6">
                        <v-textarea id="result" background-color="white" outlined name="input-7-4" auto-grow
                                    label="Italian" contenteditable="false" v-model="resultText"></v-textarea>
                    </v-col>
                </v-row>
            </v-container>
            <!--            <Fiddle/>-->
        </v-layout>
    </v-container>
</template>


<script>
    import axios from 'axios';

    export default {
        name: 'Core',
        components: {
            // Fiddle
        },
        data: () => ({
            inputText: '',
            resultText: ''
        }),
        methods: {
            translate() {
                axios.post(
                    `http://localhost:8081/api/translate`,
                    {
                        source_lang: "ru",
                        target_lang: "it",
                        text: this.inputText
                    })
                    .then((value) => {
                        this.resultText = value.data.translation
                    })
            }
        },
        watch: {
            inputText: function () {
                this.translate
            }
        }
    }

</script>

<!--<style lang="scss">-->
<!--    @import '../stylus/css/fiddle.css';-->
<!--</style>-->