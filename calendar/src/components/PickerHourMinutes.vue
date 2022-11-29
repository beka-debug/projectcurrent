<template>
    <div class="time">
        <div class="left"
        v-on:mousedown="mouseIsDown"
        v-on:mouseleave="mouseLeave"
        v-on:mouseup="mouseUp"
        v-on:mousemove="mouseMove">
            <p v-for="i of this.leftarr">{{i}}</p>
        </div>
        <div class="right"></div>
        <button @click="x()">x</button>
    </div>
</template>

<script>
export default{
    data(){
        return{
            arr1:[],
            isDown:null,
            scrollTop:null,
            startY:null
        }
    },
    computed:{
        leftarr(){
            for(let i = 0; i < 24; i++){
                if(i < 10){
                    this.arr1.push(`0${i}`)
                }
                else{
                    this.arr1.push(i)
                }
            }
            return this.arr1
        }
        
    },
    methods: {
        x(){
            console.log(this.leftarr)
            let left = document.querySelector(".left")
            left.style.background = "red"
        },
        mouseIsDown:function(e){
            let left = document.querySelector(".left")
            this.isDown = true;
            this.startY = e.pageY - left.offsetTop;
            this.scrollTop = left.scrollTop; 
            console.log("down")
        },
        mouseUp:function(e){
            this.isDown = false;
            console.log("up")
        },
        mouseLeave:function(e){
            this.isDown = false;
            console.log("leave")
            
        },

        mouseMove:function(e){
        if(this.isDown){
            e.preventDefault();
            console.log("scrol")
            let left = document.querySelector(".left")
            const y = e.pageY - left.offsetTop;
            const walkY = y - this.startY;
            left.scrollTop = this.scrollTop - walkY;
        }
      }
    },

}
</script>


<style scoped>
    .time{
        width: 200px;
        height: 400px;
        display: flex;
        align-items: center;
    }
    .left,.right{
        width: 50%;
        height: 100%;
        overflow: scroll;
    }

</style>