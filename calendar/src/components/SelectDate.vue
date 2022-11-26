<template>
    <div>
        <input maxlength="10" minlength="10" 
        type="text" placeholder="Start Date"
        v-model="fulldate"
        v-on:keyup="fdate">
        
    </div>
</template>

<script>
export default{
  data() {
    return {
        day:"",
        month:"",
        year:"",
        fulldate:""
    }
  },
  computed:{
    // date(){
    //     return `${this.day}.${this.month}.${this.year}`
    // },
    // fdate(){
    //     if(this.fulldate.length == 2 || this.fulldate.length == 5){
    //         this.fulldate += "."
    //     }
    //     return this.fulldate
    // }
  },
  methods: {
        fdate:function(e){
        if(e.keyCode != 8 && (this.fulldate.length == 2 || this.fulldate.length == 5)){
            this.fulldate += "."
        }
        if(this.fulldate.length ==10){
            this.$emit("fulldateemitter",this.fulldate)
        }
        return this.fulldate
    }
  },
}
</script>

<style scoped>
::placeholder {
   text-align: center; 
}
input:focus ::placeholder{
    content: asd;
}

</style>