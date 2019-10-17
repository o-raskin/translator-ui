<template>
    <v-container>
        <v-layout text-center wrap>
            <v-flex id="message-alert" mt-5>
                <v-row align="center" justify="center">
                    <v-alert v-if="inputText === '' && resultText === ''" dense type="info" min-width="10%" max-width="60%">
                        Just write something...
                    </v-alert>
                    <v-alert v-else-if="resultText !== ''" dense type="success" min-width="30%" max-width="50%">
                        We have some translation for you!
                    </v-alert>
                    <v-alert v-else-if="inputText !== '' && resultText === ''" dense type="info" min-width="10%" max-width="40%">
                        Hmmm...
                    </v-alert>
                </v-row>
            </v-flex>
            <v-container fluid>
                <v-row>
                    <v-col cols="12" md="6">
                        <v-textarea outlined name="input-7-4" auto-grow label="Russian" value="" v-model="inputText"
                        ></v-textarea>
                    </v-col>
                    <v-col cols="12" md="6">
                        <v-textarea outlined name="input-7-4" auto-grow label="Italian" contenteditable="false" value="123"
                                    v-model="resultText"></v-textarea>
                    </v-col>
                </v-row>
            </v-container>
        </v-layout>
    </v-container>
</template>


<script>

    export default {
        name: 'Core',
        components: {},
        data: () => ({
            inputText: '',
            resultText: ''
        }),
        methods: {}
    };

</script>