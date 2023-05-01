<template>
   <div>
    <img :src="room.image">
    <!--<p v-on:click="checked_product=index, modal_status=true">상품이름 : {{ products[index].title }}</p>-->
    <p v-on:click="$emit('openModal', room.id)">상품이름 : {{room.title}}</p>
    <!--$어쩌구는 Vue만의 특별한 변수-->
    <!--$emit() : 부모에게 메세지(openModal)를 보냄-->
    <br>
    상품가격 : {{room.price }}원
   </div> 
</template>

<script>
export default {
    //eslint-disable-next-line
    name:"Card",
    props:{
        room : Object,
    }
}
</script>

<style>

</style>