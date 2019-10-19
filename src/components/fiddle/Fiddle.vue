<template>
    <v-container>
        <svg width="200" height="200">
            <polygon :points="points"></polygon>
            <circle cx="100" cy="100" r="90"></circle>
        </svg>
    </v-container>
</template>

<script>
    import TweenLite from 'gsap/TweenLite'

    export default {
        name: "Fiddle",
        data() {
            return {
                stats: Array.apply(null, {length: 10}).map(function () {
                    return 100
                }),
                points: generatePoints(this.stats),
                sides: 10,
                minRadius: 50,
                interval: null,
                updateInterval: 500
            }
        },
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
                    {points: generatePoints(newStats)}
                )
            },
            updateInterval: function () {
                this.resetInterval()
            }
        },
        mounted: function () {
            this.resetInterval()
        },
        created() {
            let tweenMax = document.createElement('script');
            tweenMax.setAttribute('src',"https://cdnjs.cloudflare.com/ajax/libs/gsap/1.20.3/TweenMax.min.js");
            document.head.appendChild(tweenMax);
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
            }
        }
    }

    function valueToPoint(value, index, total) {
        var x = 0
        var y = -value * 0.9
        var angle = Math.PI * 2 / total * index
        var cos = Math.cos(angle)
        var sin = Math.sin(angle)
        var tx = x * cos - y * sin + 100
        var ty = x * sin + y * cos + 100
        return {x: tx, y: ty}
    }

    function generatePoints(stats) {
        var total = stats.length;
        return stats.map(function (stat, index) {
            var point = valueToPoint(stat, index, total);
            return point.x + ',' + point.y
        }).join(' ')
    }
</script>

<style scoped>
    @import './css/fiddle.css';
</style>