<template>
    <div class="position-relative" style="height: 100px">

        <!-- SQUIRREL -->
        <img src="../assets/crab_one.png" :style="squirrelMoveOne">
        <img src="../assets/crab_two.png" :style="squirrelMoveTwo">
        <img src="../assets/crab_jump.png" :style="squirrelMoveJump">

    </div>


</template>

<script>
    export default {
        name: "crab",
        props: ['frame','jumping'],

        data(){

            return {

                change : false,
            }
        },
        watch: {


            // whenever question changes, this function will run
            frame: function (newFrame) {
                if(newFrame % 2 === 0){
                    this.change ^= true;
                }
            },
        },
        computed: {



            squirrelMoveOne: function(){
                let style = 'position:absolute;bottom:0;left:0;width:100px;height:100px;bottom:0;';

                ( this.change  && !this.jumping  ) ? style += 'opacity:1;' : style += 'opacity:0;';

                return style
            },


            squirrelMoveJump: function(){
                let style = 'position:absolute;bottom:0;left:0;width:100px;height:100px;';

                ( this.jumping ) ? style += 'opacity:1;' : style += 'opacity:0;';

                return style


            },

            squirrelMoveTwo: function(){
                let style = 'position:absolute;bottom:0;left:0;width:100px;height:100px;';

                ( !this.change && !this.jumping ) ? style += 'opacity:1;' : style += 'opacity:0;';

                return style


            }
        }
    }
</script>

<style scoped>

</style>
