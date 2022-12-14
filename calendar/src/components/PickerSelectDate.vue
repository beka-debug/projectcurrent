<template>
    <div>
        <div 
        @click="clickstart()"
        v-bind:class="{ st: !clicked, st2: clicked }"
        
        >Start Date</div>
        <input tabindex="1" v-on:keyup="fulldt()" type="date" v-model="dt" placeholder="start date">
    </div>
</template>

<script>
export default{
    data(){
        return {
            dt:"",
            clicked:false
        }
    },
   methods: {
    fulldt(){
        console.log(this.dt.length, this.dt, this.dt[0])
        if(this.dt.length == 10 && this.dt[0] != "0"){
            this.$emit("fulldateemitter",this.dt)
        }
    },
    clickstart(){ 
        this.clicked = true
    }
   },
}
</script>

<style scoped>

input{
    width: 100%;
    box-sizing: border-box;
    text-align: center;
    padding: 5px;
    border-radius: 5px;
    border:none;
    background-color: #f3f3f3;
    margin: 2px 0px;
    
}
input:focus{
    outline: none;
  box-shadow: none!important;
}
.st{
    width: 230px;
    background-color: #f3f3f3;
    border-radius: 5px;
    padding: 5px;
    box-sizing: border-box;
    position: absolute;
}
.st2{
    border: 1px solid #82C341;
    box-sizing: border-box;
    position: absolute;
    width: 232px;
    margin-left: -1px;
    padding: 5px 8px;
    border-radius: 6px;
    content: '';
    z-index: -1;
    text-align: center;
    
}
input[type="date"]::-webkit-inner-spin-button,
input[type="date"]::-webkit-calendar-picker-indicator {
    display: none;
    -webkit-appearance: none;
}
input[type=date]::-webkit-datetime-edit {
    color: #82C341;
} 
</style>