<template>
    <v-app>
        <v-app-bar class="primary" app>
            <v-toolbar-title class="white--text headline text-uppercase">
                <span class="font-weight-light">Translator </span> -
                <span class="font-weight-bold">UI</span>
                <span class="overline"> 1.0</span>
            </v-toolbar-title>
        </v-app-bar>

        <v-content class="darken-2">
            <Core/>
        </v-content>
      <Footer class="grey darken-5"/>
    </v-app>
</template>

<script>
    import Core from './components/Core';
    import Footer from './components/Footer';

    export default {
        name: 'App',
        components: {
            Core,
            Footer
        },
        data: () => ({})
    };

</script>
