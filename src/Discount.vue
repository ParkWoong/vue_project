<template>
    <div class="discount">
     <h4>지금 결제하면 {{ discountedPercent }}% 할인</h4>
    </div>
</template>

<script>
export default {
    //eslint-disable-next-line
    name:'Discount',
    props : {
        discountedPercent:Number
    }
}
</script>

<style>

</style>

