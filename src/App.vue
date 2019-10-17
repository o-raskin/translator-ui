<template>
    <v-app>
        <v-app-bar app>
            <v-toolbar-title class="headline text-uppercase">
                <span class="font-weight-light">Translator - </span>
                <span class="font-weight-bold">UI</span>
            </v-toolbar-title>
        </v-app-bar>

        <v-content>
            <Core/>

        </v-content>
      <Footer/>
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
