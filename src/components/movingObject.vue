<template>
    <div ref="obstacle" :style="styledObject">
        <img src="../assets/plastic-bag.png" class="img-fluid" :class="{'plastic-bag-al': delay_loops !== 1, 'plastic-bag': delay_loops === 1,  }" >
    </div>

</template>

<script>
    export default {
        name: "obstacle",
        props: ['scenewidth','frame','framerate','passref','squirellposition','delay_frames','delay_loops','speed_coefficient'],

        data() {
            return {

                startFrame : 0,
                lastFrame : 0,
                width: 100,
                delay_gap: 0,
                completedLoops :0,


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

                let style='position:absolute;width: '+this.width+'px;height:70px;bottom:10%;z-index: 9000;';

                let speed =  parseInt(this.framerate * this.speed_coefficient );
                let left = parseInt((this.scenewidth*1.1)/speed)

                left = parseInt(this.lastFrame * left)-(this.width)

                //console.log(left,'left',speed,'speed','lastfrmae',this.lastFrame)

                style += 'right:'+left+'px;'


                if( this.$refs.obstacle) {

                    console.log('this.squirellposition.bottom < this.$refs.obstacle.getBoundingClientRect().top',
                        this.squirellposition.bottom , this.$refs.obstacle.getBoundingClientRect().top)
                    if (this.squirellposition.right > this.$refs.obstacle.getBoundingClientRect().left &&
                        this.squirellposition.left < this.$refs.obstacle.getBoundingClientRect().right &&
                        this.squirellposition.bottom >  this.$refs.obstacle.getBoundingClientRect().top

                    )

                    {
                        this.$emit('caught')
                    }
                }

                return style;

            },


        }

    }
</script>

<style scoped>

</style>
