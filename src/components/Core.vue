<template>
    <v-container fluid align="center" justify="center" ml-0 mr-0>
        <v-layout text-center wrap>
            <v-flex mt-5>
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
                                    :label="source.title" counter="true" v-model="inputText" v-on:input="translate"
                                    :value="source.tag">
                        </v-textarea>
                    </v-col>
                    <v-col md="1">
                        <v-btn v-on:click="swap" text icon color="primary">
                            <v-icon>mdi-swap-horizontal</v-icon>
                        </v-btn>
                    </v-col>
                    <v-col md="5">
                        <v-textarea id="result" background-color="white" outlined auto-grow :label="target.title"
                                    contenteditable="false" hide-details v-model="resultText" :value="target.tag" disabled>
                        </v-textarea>
                    </v-col>
                </v-row>
                <v-row>
                    <svg width="200" height="200">
                        <polygon :points="this.points"></polygon>
                    </svg>
                </v-row>
            </v-flex>
        </v-layout>
    </v-container>
</template>


<script>
    import axios from 'axios';
    import _ from 'lodash';
    import TweenLite from 'gsap';

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
            ],
            function () {
                var defaultSides = 10
                var stats = Array.apply(null, { length: defaultSides })
                    .map(function () { return 100 })
                return {
                    stats: stats,
                    points: generatePoints(stats),
                    sides: defaultSides,
                    minRadius: 50,
                    interval: null,
                    updateInterval: 500
                }
            }
        }),
        watch: {
            sides: function (newSides, oldSides) {
                var sidesDifference = newSides - oldSides
                if (sidesDifference > 0) {
                    for (var i = 1; i <= sidesDifference; i++) {
                        this.stats.push(this.newRandomValue())
                    }
                } else {
                    var absoluteSidesDifference = Math.abs(sidesDifference)
                    for (var j = 1; j <= absoluteSidesDifference; j++) {
                        this.stats.shift()
                    }
                }
            },
            stats: function (newStats) {
                TweenLite.to(
                    this.$data,
                    this.updateInterval / 1000,
                    { points: generatePoints(newStats) }
                )
            },
            updateInterval: function () {
                this.resetInterval()
            }
        },
        mounted: function () {
            this.resetInterval()
        },
        methods: {
            randomizeStats: function () {
                var vm = this
                this.stats = this.stats.map(function () {
                    return vm.newRandomValue()
                })
            },
            newRandomValue: function () {
                return Math.ceil(this.minRadius + Math.random() * (100 - this.minRadius))
            },
            resetInterval: function () {
                var vm = this
                clearInterval(this.interval)
                this.randomizeStats()
                this.interval = setInterval(function () {
                    vm.randomizeStats()
                }, this.updateInterval)
            },
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
                this.translate()
            }
        }
    }
    function valueToPoint (value, index, total) {
        var x     = 0
        var y     = -value * 0.9
        var angle = Math.PI * 2 / total * index
        var cos   = Math.cos(angle)
        var sin   = Math.sin(angle)
        var tx    = x * cos - y * sin + 100
        var ty    = x * sin + y * cos + 100
        return { x: tx, y: ty }
    }

    function generatePoints (stats) {
        var total = stats.length
        return stats.map(function (stat, index) {
            var point = valueToPoint(stat, index, total)
            return point.x + ',' + point.y
        }).join(' ')
    }
</script>
<style>
    svg { display: block; }
    polygon { fill: deepskyblue; }
    input[type="range"] {
        display: block;
        width: 100%;
        margin-bottom: 15px;
    }
</style>