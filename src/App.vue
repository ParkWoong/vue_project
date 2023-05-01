<template>
  <div class="menu">
    <a v-for="(key,index) in menu" :key="index">{{ key }}</a>
  </div>
  
  <!--모달 컴포넌트-->
  <!-- 컴포넌트를 사용할 때는 부모의 데이터를 자식이 쓸 수 있게 해줘야 한다.-->
  <!--props-->
  <!--데이터보내고 / 등록하고 / 쓰기-->
  <!--custom event-->
  <!--자식 컴포넌트에서 부모 컴포넌트의 데이터를 변경하고 싶을때-->
  <Transition name="fade"> 
    <!-- <div class="start" :class="{end : modal_status}"> </div> -->
    <!--동적으로 class 이름 설정해주기 {클래스 이름 : 조건이 true일때 class 이름 동적으로 지어주기}-->
    <!--애니메이션 주기!--> 
    <Modal @closeModal="modal_status=false" :products="products[checked_product]" :modal_status="modal_status"></Modal>
  </Transition>






  <!--이 HTML이 너무 길어서 한글자로 축약하고 싶다 -> Component 만들기-->
  <Discount v-if="showDiscount" :discountedPercent="discountedPercent"></Discount>

  <button @click="priceSort">가격순 정렬</button>
  <button @click="sortBack">되돌리기</button>


  
  <!--컴포넌트의 반복문-->
  <Card @openModal="modal_status=true ; checked_product = $event"  v-for="(key,index) in products" :key="index" :room="products[index]"></Card>
  <!--자식한테 받아온 openModal이라는 메세지(이벤트)가 발생하면 실행-->
  <!--@openModal="$event" : 자식이 보낸 데이터는 $event로 받을 수 있음-->

  

  <!-- <div>
    <h4 v-for="room in products" :key="room">{{room }}</h4>
    <p>{{ price }} 만원</p> 데이터 바인딩 : Vue의 실시간 렌더링 기능(변경기능)을 이용하기 위해
    <hr>
    <hr>
    <h4>{{ products[0] }}</h4>
    <button @click="call1">허위신고버튼</button> <p>{{ record_call[0] }}</p>
    <h4>{{ products[1] }}</h4>
    <button @click="call2">허위신고버튼</button> <p>{{ record_call[1] }}</p>
    <h4>{{ products[2] }}</h4>
    <button @click="call3">허위신고버튼</button> <p>{{ record_call[2] }}</p>
  </div> -->
</template>

<script>

import datas from "./assets/data";
import Discount from "./Discount.vue";
import Modal from "./Modal.vue";
import Card from "./components/Card.vue";


export default {
  name: 'App',
  data(){ // 데이터 바인딩을 위한 데이터 보관함
   return{
    products_original : [...datas],
    // array/object 데이터의 각각 별개의 사본을 만들기, sort()와 같은 영구적으로 변하는 함수의 영향X
    checked_product : 0,
    products : datas,
    modal_status : false,
    record_call : [0,0,0],
    menu : ["Home", "Products", "About"],
    showDiscount : true,
    discountedPercent : 20,
   }
  },
  methods:{
    call1(){
      this.record_call[0] ++;
    },
    call2(){
      this.record_call[1] ++;
    },
    call3(){
      this.record_call[2] ++;
    },
    priceSort(){
      this.products.sort(function(a,b){ //sort는 원본을 영구적으로 변형, map(), filter()은 원본을 보존시켜줌
        return a.price-b.price
      });
    },
    sortBack(){
      this.products.sort(function(a,b){
        return a.id-b.id;
      })

      //this.products = [...this.products_original]
      //array의 경우 = 기호는 값을 넣어주세요가 아닌 값을 공유해주세요 라는 의미
      //따라서 어느 순간 두 array의 값들이 똑같아짐
      //마찬가지로 각각의 사본을 계속해서 넣어줘야지 수정이 가능해짐
    },
  },
  mounted(){
    // LifeCycleHook
    // App.vue가 마운트 되고 나서
    // 서버에서 데이터 가져올 때고 LifeCycleHook 코드 안에서 aJax로 코드를 짜준다.
    // 재랜더링은 update()로 할 수 있다.

    // 변수를 사용하지 않았다고 오류가 날때는 옆에 주석처리를 해주자~
    let timer = setInterval(()=>{ //eslint-disable-line no-unused-vars
      this.discountedPercent--;
      if(this.discountedPercent == 18){
        this.showDiscount = false;
        clearInterval(timer);
      }
    }, 1000);
  },
  beforeUpdate(){
    
  },
  components: {
    Discount : Discount,
    Modal : Modal,
    Card : Card,
  }
}
</script>

<style>

body {
  margin : 0;
}
div {
  box-sizing:border-box;
}
.black_bg {
  width: 100%; height: 100%;
  background: rgba(0,0,0,0.5);
  position: fixed; padding: 20px;
}
.white_bg {
  width: 100%; background: white;
  border-radius: 8px;
  padding:20px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.menu{
  background : rgb(14, 14, 65);
  padding : 15px;
  border-radius : 5px;
}

.menu a{
  color : white;
  padding : 10px;
}

.discount{
  background: gray;
  padding:10px;
  margin: 10px;
  border-radius: 5px;
}
/* .start{
  opacity: 0;
  transition : all 1s; 
}
.end{
  opacity: 1;
} */

/* Transition css적용하기 */
.fade-enter-from{opacity: 0;} /* 시작 */
.fade-enter-active{transition: all 1s;}
.fade-enter-to{opacity:1;} /* 끝 */

/* Transition 퇴장시 css적용하기 */
.fade-leave-from{opacity: 1;} /* 시작 */
.fade-leave-active{transition: all 1s;}
.fade-leave-to{opacity:0;} /* 끝 */

</style>
