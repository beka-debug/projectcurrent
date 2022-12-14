<template>
    <div class="x">
        <input type="time">
    </div>
</template>

<script>
export default{

}
</script>

<style scoped>
input{
    width: 100%;
    box-sizing: border-box;
    padding: 0px 10px;
}
input[type="time"] {
    background: #fff;
    
}

input[type="time"]:before {
    content: 'HH:MM';
    color: #9d9d9d;
    position: absolute;
    background: #fff;

}

input[type="time"]:focus:before {
    width: 0;
    content: '';
}
input[type="time"]::-webkit-calendar-picker-indicator {
    background: none;
}
.x{
    padding: 0px 20px;
}
</style>