<template>
    <div class="years">
        <ol class="all_years">
      <li 
      
      v-for="year,index in this.years" :key="year"
      @click="chooseYear(index)" ref="refWord">{{year }}</li>
    </ol>
    
    </div>
</template>

<script>
let years = []
let dt = new Date().getFullYear()
for(let i = dt - 11; i <= dt; i++){
    years.push(i)
}

export default{
    name: "CalendarYears",
    data(){
        return{
            arr:[]
        }
        
    },
    computed: {
      years() {
        return years;
      }
    },
  props:{
    dropyears:{
        type:Array
    }
  },
  methods: {
    chooseYear(index){
        console.log(index)
          let chsyear = this.$refs.refWord[index].innerText
          let info = {
            year:chsyear,
            show:false,
            index:index
          }
         this.$emit("chooseyearemitter",info,)
         console.log(info)
    },

  },
}


</script>

<style scoped>

.years{
    width: 300px;
    height: 250px;
    background-color: red;

    
}
.all_years {
    display: flex;
    flex-wrap: wrap;
    align-content:space-around;    
    height: 100%;
  }

  li{
    width: 33.3%;
  }

</style>