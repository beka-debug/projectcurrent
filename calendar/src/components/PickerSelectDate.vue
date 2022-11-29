<template>
    <div>
        <input v-on:keyup="fulldt()" type="date" v-model="dt" placeholder="start date">
    </div>
</template>

<script>
export default{
    data(){
        return {
            dt:""
        }
    },
   methods: {
    fulldt(){
        console.log(this.dt.length, this.dt, this.dt[0])
        if(this.dt.length == 10 && this.dt[0] != "0"){
            this.$emit("fulldateemitter",this.dt)
        }
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
}
</style>