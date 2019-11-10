<template>
    <div ref="obstacle"  :style="styledObject">
        <img src="../assets/tree_one_c.png" class="img-fluid" v-if="tree_numb === 1">
        <img src="../assets/tree_two_c.png" class="img-fluid" v-if="tree_numb === 2">
        <img src="../assets/tree_three_c.png" class="img-fluid" v-if="tree_numb === 3">

    </div>

</template>

<script>
    export default {
        name: "backgound",
        props: ['scenewidth','bottom','frame','framerate','squirellposition','delay_frames','delay_loops','scale','speed_coefficient','tree_numb'],

        data() {
            return {

                startFrame : 0,
                lastFrame : 0,
                width: 100,
                delay_gap: 0,
                completedLoops :0,
                names : ['../assets/tree_one.png','../assets/tree_two.png','../assets/tree_three.png']

            }
        },
        mounted(){

        },

        watch: {

            frame: function(newframe) {

                let speed = parseInt(this.framerate * this.speed_coefficient);

                if((newframe - this.delay_frames) % speed === 0){
                    this.completedLoops++
                    console.log('complete loops',this.completedLoops)
                }

                if(this.completedLoops % this.delay_loops === 0){
                    this.lastFrame = ((newframe - this.delay_frames) % speed);
                } else {

                    this.lastFrame = 0
                }
            }
        },

        computed: {

            styledObject: function () {

                let style='position:absolute;width: '+this.width+'px;height:70px;bottom:'+this.bottom+'%;z-index: 9000;';
                style+='transform: scale('+this.scale+');';

                let speed =  parseInt(this.framerate * this.speed_coefficient );
                let left = parseInt((this.scenewidth*1.4)/speed)

                left = parseInt(this.lastFrame * left)-(this.width)

                //console.log(left,'left',speed,'speed','lastfrmae',this.lastFrame)

                style += 'right:'+left+'px;'


                if( this.$refs.obstacle) {
                    if (this.squirellposition.left > this.$refs.obstacle.getBoundingClientRect().left
                        && this.squirellposition.left >  (this.$refs.obstacle.getBoundingClientRect().left + this.width)

                    ) {

                        //this.$emit('caught')
                    }
                }

                return style;

            },


        }

    }
</script>

<style scoped>

</style>
