<template>
    <div class="time">
        
        <div class="left sc"
        v-on:mousedown="mouseIsDown"
        v-on:mouseleave="mouseLeave"
        v-on:mouseup="mouseUp"
        v-on:mousemove="mouseMove"
        >
            <p 
            
            v-on:click="scrollauto"
            @click="chooseHour(index);"
            ref="refWord"
            v-for="i,index of this.leftarr"
            v-bind:class="{hid: String(i).slice(1,3) < 0 || i > 23,'t':true}"
            >{{i}}</p>
        </div>
        <div class="middle"></div>
        <div class="right sc"
        v-on:mousedown="mouseIsDown1"
        v-on:mouseleave="mouseLeave1"
        v-on:mouseup="mouseUp1"
        v-on:mousemove="mouseMove1">
            <p 
            v-on:click="scrollauto2"
            @click="chooseMinute(index)"
            ref="refWord"
            v-for="i,index of this.rightarr"
            v-bind:class="{hid: String(i).slice(1,3) < 0 || i > 59}">{{i}}
        </p>
        </div>
        
    </div>
    
</template>

<script>
export default{
    data(){
        return{
            arr1:[],
            arr2:[],
            isDown:null,
            scrollTop:null,
            startY:null,
        }
    },

    computed:{
        leftarr(){
            for(let i = -3; i < 27; i++){
                if(i < 10){
                    this.arr1.push(`0${i}`)
                }
                else{
                    this.arr1.push(i)
                }
            }
            return this.arr1
        },
        rightarr(){
            for(let i = -3; i < 63; i++){
                
                if(i < 10){
                    this.arr2.push(`0${i}`)
                }
                else{
                    this.arr2.push(i)
                }
            }
            return this.arr2
        }

        
    },
    mounted() {
       this.q(() => {
  console.log('The user has stopped scrolling');

});
this.q1(()=>{
    console.log('The user has stopped scrolling');
})
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
          //  console.log("down")
        },
        mouseUp:function(e){
            this.isDown = false;
           // console.log("up")
        },
        mouseLeave:function(e){
            this.isDown = false;
           // console.log("leave")
            
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
      },
      mouseIsDown1:function(e){
            let right = document.querySelector(".right")
            this.isDown = true;
            this.startY = e.pageY - right.offsetTop;
            this.scrollTop = right.scrollTop; 
           // console.log("down")
        },

        mouseUp1:function(e){
            this.isDown = false;
           // console.log("up")
        },
        mouseLeave1:function(e){
            this.isDown = false;
           // console.log("leave")
            
        },

        mouseMove1:function(e){
        if(this.isDown){
            e.preventDefault();
            console.log("scrol")
            let right = document.querySelector(".right")
            const y = e.pageY - right.offsetTop;
            const walkY = y - this.startY;
            right.scrollTop = this.scrollTop - walkY;
            console.log(right.scrollTop)
        }
      },
        q(callback){
    let isScrolling;
    let left = document.querySelector(".left")
    left.addEventListener("scroll",function(e){
        
        clearTimeout(isScrolling);
        isScrolling = setTimeout(() => {
        callback();
        e.target.scrollTop = 33.7 * parseInt(e.target.scrollTop/32.7)
        let scrolledHour =  parseInt(e.target.scrollTop/33)
       // this.$emit("scrollhouremitter",scrolledHour)
        
        }, 250);
    },false)
},
q1(callback){
    let isScrolling;
    let right = document.querySelector(".right")
    right.addEventListener("scroll",function(e){
        clearTimeout(isScrolling);
        isScrolling = setTimeout(() => {
        callback();
        e.target.scrollTop = 33.7 * parseInt(e.target.scrollTop/32.7)


        }, 250);
    },false)
},



      scrollauto:function(e){

        let left1 = document.querySelector(".left")
        let distancefromtop = left1.offsetTop
        let divheight = 212
        let elementheight = 34
        let count = 1
        let mid1 = distancefromtop + divheight/2 + elementheight/2
        let mid2 = distancefromtop + divheight/2 - elementheight/2
        let stop = 0
        //253
        //console.log(mid1,"mid1")
        //console.log(mid2,"mid2")
        //console.log(e.layerY,"layer")
        if(e.layerY <= mid1){
            
          count = parseInt((mid1 - e.layerY) / elementheight)
          //left1.scrollTop -= elementheight * count
          let dif = e.layerY- left1.offsetTop
          console.log(1,dif)
         //  console.log(dif,"diff")
            // left1.scrollTop += elementheight * count
             let counter = 0
             
             let inter = setInterval(() => {
                 left1.scrollTop -= 31.7/3
                 counter++
                 if(dif <= 11){
                     stop = 9
                 }
                 else if (dif<=45){
                     stop = 6
                 }
                 else if(dif <=80){
                     stop = 3
                     console.log("asdasdasdasdasd")
                 }
                 else{
                    clearInterval(inter)
                    console.log("done")
                    left1.scrollTop += 31.7/3
                    console.log(counter)
                    //left1.scrollTop += 31.7/3
                 }
                 if(counter == stop){
                 clearInterval(inter)
                 }
             }, 50);
          //console.log("2222222",e.layerY-left1.offsetTop)
        }
        else if(e.layerY >= mid2){
            console.log(2)
            count = parseInt((e.layerY - mid2) / elementheight)
            let dif = e.layerY- left1.offsetTop
            console.log(dif)
           // left1.scrollTop += elementheight * count
            let counter = 0
            let inter = setInterval(() => {
                left1.scrollTop += 33.7/3
                counter++
                if(dif < 110){
                    stop = 1
                    left1.scrollTop -= 33.7/3
                }
                if(dif <= 145){
                    stop = 3
                }
                else if (dif >= 145 && dif <= 180){
                    stop = 6
                }
                else if(dif > 180){
                    stop = 9
                }
                if(counter == stop){
                clearInterval(inter)
                }
            }, 50);
            // console.log("2222222",e.layerY,e.layerY- left1.offsetTop)
        }
      },
      scrollauto2:function(e){

let right1 = document.querySelector(".right")
let distancefromtop = right1.offsetTop
let divheight = 212
let elementheight = 34
let count = 1
let mid1 = distancefromtop + divheight/2 + elementheight/2
let mid2 = distancefromtop + divheight/2 - elementheight/2
let stop = 0

if(e.layerY <= mid1){
  count = parseInt((mid1 - e.layerY) / elementheight)
  //left1.scrollTop -= elementheight * count
  let dif = e.layerY- right1.offsetTop
    
    // left1.scrollTop += elementheight * count
     let counter = 0
     let inter = setInterval(() => {
        right1.scrollTop -= 31.7/3
         counter++
         if(dif <= 11){
             stop = 9
         }
         else if (dif<=45){
             stop = 6
         }
         else if(dif <=80){
             stop = 3
         }
         if(counter == stop){
         clearInterval(inter)
         }
     }, 50);
  console.log("2222222",e.layerY-right1.offsetTop)
}
else if(e.layerY >= mid2){
    count = parseInt((e.layerY - mid2) / elementheight)
    let dif = e.layerY- right1.offsetTop
    
   // left1.scrollTop += elementheight * count
    let counter = 0
    let inter = setInterval(() => {
        right1.scrollTop += 33.7/3
        counter++
        if(dif <= 145){
            stop = 3
        }
        else if (dif >= 145 && dif <= 180){
            stop = 6
        }
        else if(dif > 180){
            stop = 9
        }
        if(counter == stop){
        clearInterval(inter)
        }
    }, 50);
    // console.log("2222222",e.layerY,e.layerY- left1.offsetTop)
}
},
      chooseHour(index){
        let hour = this.$refs.refWord[index].innerText
        this.$emit("choosehouremitter",Number(hour))
    },
    chooseMinute(index){
        let minute = this.$refs.refWord[index].innerText
        this.$emit("chooseminuteemitter",minute)
    }

    },


}
</script>


<style scoped>
    .time{
        width: 130px;
        
        height: 212px;
        display: flex;
        align-items: center;
        
    }
    .left,.right{
        width: 50%;
        height: 100%;
        font-size: 14px;
        overflow: scroll;
    }
    .left::-webkit-scrollbar {
  display: none;
}
.right::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE and Edge */
.left, .right {
  -ms-overflow-style: none;
}

.left p{
    margin: 17px 0px;
}
.right p{
    margin: 17px 0px;
}
.hid{
    background-color: red;
    margin: 10px !important;
    pointer-events: none !important;
    visibility: hidden;
    
}
.middle{
    width: 90px;
    height: 37px;
    border-top: 1px solid black;
    border-bottom: 1px solid black;
    z-index: -1;
    position: absolute;
    margin-left: 20px;
}
</style>