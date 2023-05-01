<template>
  <div class="black_bg" v-if="modal_status==true">
    <div class="white_bg">
      <img :src="products.image">
      <h4>{{ products.title }}</h4>
      <p>{{ products.content }}</p>
      <input @input="month = $event.target.value" id="puttedMonth">
      <!--<input v-model.number="month"> : 위의 문법이랑 동일, 좀 더 간편-->
      <!-- input 이벤트 : 뭔가 입력할 때마다 실행-->
      <!--event.target.value : 현재 입력되고 있는 input의 값을 의미-->
      <!--사용자가 <input>에 입력한 것은 전부 문자자료형-->
      <br>
      <select v-model="month">
        <option>1</option>
        <option>2</option>
        <option>3</option>
      </select>
      <p>{{ month }}개월 선택함 : {{ products.price * month }}원</p>
       <button @click="$emit('closeModal')">모달창 닫기</button>
       <!--(주의) props는 read-only임, 받아온거 수정하면 큰일남-->
    </div>
  </div>
</template>

<script>
export default {
    //eslint-disable-next-line
    name:"Modal",
    data(){
      return{
        month:1,
        //watcher(data를 감시하는 함수), input을 받을 때는 거의 필수
      }
    },
    props:{
        products : Object,
        checked_product : Number,
        modal_status : Boolean,
    },
    beforeUpdate(){
      if(this.month==2){
        alert("2이상의 값을 입력하여야 합니다.");
        document.getElementById("#puttedMonth").value = 4;
        this.month = 3;
      }
    },
    watch : {
      // month(변경 될 데이터, 변경 전 데이터)
      month(a){
        // month 라는 데이터가 변할 때마다 여겨있는 코드 실행됨
        // 사용자가 month를 글자로 입력하면 경고문을 띄워주셈
        // 다양한 유효성 검사는 Vue 전용 form validation 라이브러리를 설치하면 편리
        if(a>13){
          alert('13이상 입력 불가합니다.')
        }
        if(isNaN(a)){
          alert('숫자만 입력 가능합니다.')
          document.getElementById("#puttedMonth").value=" ";
          //이거 다시 해보기!
          this.month = 1;
        }
      }
    }
}
</script>

<style>

</style>