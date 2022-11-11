<template>
    <div 

    class="months">
        <ol class="all-months">
      <li 
      
      v-for="month, index in months" :key="month"
      @click="chooseMonth(index)" ref="refWord">{{ month }}</li>
    </ol>
    </div>
</template>

<script>
const MONTHS = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov","Dec"]
export default{
    name: "CalendarMonths",

    data(){
        return{
            show:false,
            info:{}
        }
        
    },
    computed: {
      months() {
        return MONTHS;
      }
    },
    props:{
      curmonths:{
             type:Array
       }
    },
    methods: {
        chooseMonth(index){
          let chsmonth = this.$refs.refWord[index].innerText
          let info = {
            month:chsmonth,
            show:false,
            index:index
          }
         this.$emit("choosemonth",info)
         //console.log("emited");
        }
    },
    // mounted() {
    //   console.log(this.curmonths)
    // },
}
</script>

<style scoped>
.months{
    width: 300px;
    height: 250px;
    background-color: white;
    margin-top: -240px;
    position: absolute;
    
}
.all-months {
    /* display: grid;
    grid-template-columns: repeat(3, 1fr);
    background-color: red !important; */
    display: flex;
    flex-wrap: wrap;
    align-content:space-around;
    
    height: 100%;
  }
  li{
    width: 33.3%;
  }
</style>