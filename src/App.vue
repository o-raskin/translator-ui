<template>
    <v-app>
        <v-app-bar class="primary" app>
            <v-toolbar-title class="white--text headline text-uppercase">
                <span class="font-weight-light">{{strings[0]}} </span> -
                <span class="font-weight-bold">{{strings[1]}} </span>
                <span class="overline">{{strings[2]}}</span>
            </v-toolbar-title>
        </v-app-bar>
        <v-content class="darken-2">
            <Core/>
        </v-content>
      <Footer class="grey darken-5"/>
    </v-app>
</template>

<script>
    import Core from './components/core/Core';
    import Footer from './components/footer/Footer';

    export default {
        name: 'App',
        components: {
            Core,
            Footer
        },
        data: () => ({
            strings: [
                'Translator',
                'UI',
                '2.0'
            ]
        })
    }
</script>
