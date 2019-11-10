<template>


    <div id="app" class="bg-white row mx-auto w-100">
        <div class="col-12 px-0 my-auto">
            <div class="row mx-auto rounded bg-white">
                <div class="col-12 text-left">
                    <a class="h3 my-0 d-block">
                        Points: {{parseInt(frame/10)}}
                    </a>
                    <a class="h5">
                        Your record: {{parseInt(record/10)}}
                    </a>
                </div>
                <div class="col-12 position-relative shadow my-auto game-screen"
                     ref="gameMonitor">

                    <obstacle :speed_coefficient="speedNow" :delay_loops="1" :delay_frames="0" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></obstacle>
                    <obstacle :speed_coefficient="speedNow" :delay_loops="3" :delay_frames="40" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></obstacle>

                    <background :tree_numb="3" :speed_coefficient="13" :scale="0.5"  :bottom="45" :delay_loops="1" :delay_frames="200" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></background>
                    <background :tree_numb="3" :speed_coefficient="13" :scale="0.6" :bottom="45" :delay_loops="1" :delay_frames="500" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></background>

                    <background :tree_numb="1" :speed_coefficient="12" :scale="1" :bottom="40" :delay_loops="1" :delay_frames="0" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></background>
                    <background :tree_numb="2" :speed_coefficient="12" :scale="1.2"  :bottom="41" :delay_loops="1" :delay_frames="100" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></background>

                    <background :tree_numb="1" :speed_coefficient="12" :scale="0.9" :bottom="40" :delay_loops="1" :delay_frames="300" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></background>
                    <background :tree_numb="2" :speed_coefficient="12" :scale="0.8" :bottom="41" :delay_loops="1" :delay_frames="400" v-on:caught="obstacleError()" :squirellposition="squirellPosition" :passref="'obstacle'" :framerate="frameRate" :scenewidth="gameWidth" :frame="frame"></background>
                    <!-- SQUIRREL -->
                    <div ref="squirrel" :style="squirrelMotion()" :class="{'collapse-squirrel': errors }" >
                        <crab :jumping="jumping" :frame="frame"></crab>
                    </div>
                    <div class="position-absolute row w-100 h-100 game-over-container" v-if="errors > 0">
                        <div class="col-12 my-auto text-center">
                            <h1 class="display-2 text-danger">GAME OVER</h1>
                            <a class="play-again-button" v-on:click="restart()">
                                Play Again
                            </a>
                        </div>
                    </div>
                </div>
                <div class="col-12 text-right bg-white">
                    <a class="text-muted small">
                        GeorgeTheVueCrab by Raffobaffo
                    </a>

                </div>
            </div>
        </div>

    </div>

</template>

<script>

    import crab from './components/crab.vue'
    import obstacle from './components/movingObject.vue'
    import background from './components/backgound.vue'

    import './assets/style.css'


    export default {
        name: "app",
        components: {
            crab,
            obstacle,
            background
        },

        data() {
            return {

                time : {
                    start: performance.now(),
                    last :0,
                    total: 1000
                },
                frameRate : 40,
                frame : 0,
                gameWidth : 0,
                jumping_from: 0,
                jumping : false,
                jumpHeight: 1,
                level:1,
                errors:0,
                squirellPosition : {top:0,left:0},
                pause : false,
                speedNow : 3,
                record : 0

            }
        },

        mounted(){

            window.addEventListener("keypress", function(e) {

                if(e.keyCode === 32){
                    this.jumping = true;
                    e.preventDefault();
                }
            }.bind(this));

            window.addEventListener("keyup", function(e) {

                if(e.keyCode === 32){
                    this.jumping = false;
                    e.preventDefault();
                }
            }.bind(this));

            this.gameWidth = this.$refs.gameMonitor.clientWidth;
            this.squirellPosition = this.$refs.squirrel.getBoundingClientRect();

            this.step(0)

        },
        watch: {


            // whenever question changes, this function will run
            frame: function (newFrame) {


                if(newFrame > this.record){
                    this.record = newFrame;
                }

                if(this.jumping) {

                    if (this.jumping_from < 25 && this.jumping_from > -1) {
                        this.jumping_from++;
                        this.jumpHeight = 3
                    } else {

                        if(this.jumping_from >= 25 ){
                            this.jumping_from = -10;
                            this.jumpHeight = 1;
                        } else {

                            this.jumping_from++;
                        }
                    }

                } else {

                    this.jumping_from = 0;
                    this.jumpHeight = 1;
                }

                this.squirellPosition = this.$refs.squirrel.getBoundingClientRect();
            },
        },

        computed: {

            timingConstant: function() {
                  return 1000/ this.frameRate
            },
        },

        methods:{


            restart(){

                this.errors = 0;
                this.frame = 0;
                this.step(0);
            },

            obstacleError(){
                this.errors++;
            },

            squirrelMotion(){

                let style = 'position:absolute;left:3em;width:100px;height:100px;z-index:10000;';
                style += 'bottom:'+(this.jumpHeight*10)+'%;'
                return style
            },

            step(now) {

                if(parseInt(this.time.last) + this.timingConstant < parseInt(now)){
                    this.time.last = now;
                    this.frame++
                }

                if(this.errors < 1 && !this.pause) {
                    requestAnimationFrame(this.step)
                }
            },
        }
    }
</script>

<style scoped>

</style>
