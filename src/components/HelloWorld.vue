<template>
  <div>
    <h1>watch</h1>
    <span> {{ cnt1 }}</span>
    <button type="button" @click="updateCnt1">cnt1 ++</button>

    <h1>computed</h1>
    <p>age : {{ age }}</p>
    <button type="button" @click="age--">age -</button>
    <button type="button" @click="age++">age +</button>
    <p>age > 17 ? {{ can }}</p>

    <h1>watch 와 computed 비교</h1>
    <label for="">기본 숫자 : <input type="number" v-model.number.lazy="num"></label>
    <p>제곱 computed : {{ squareC }}, 제곱 watch : {{ squareW }}</p>
    
  </div>
</template>

<script>
import { ref, watch, computed } from 'vue'
export default {
  name: 'HelloWorld',
  setup(){
    /** watch */
    const cnt1 = ref(0);

    const updateCnt1 = () => {
      cnt1.value++
    }

    /**
     * 지정한 변수값의 변화 감시. 값이 변경되기 이전 값 참조 가능
     */
    watch(
      cnt1,
      (cur, prev) => {
        console.log('composition API Watch : ' + prev + '==>' + cur);
      },
      {
        immediate: true, //이 옵션을 true로 설정해주면 변수의 처음 값도 볼 수 있음
      }
    )


    /** computed */
    const age = ref(17);

    /** 
     * vue가 가지는 함수형 속성.
     * computed의 파라미터는 로직을 가지고 값을 계산한 후 리턴하는 함수
     * 연결되는 data가 변경되면 자동으로 새로운 값을 계산하고 화면에 반영
    */
    const can = computed(() => {
      return age.value >= 18 ? 'yes' : 'no';
    })


    /** computed와 watch 비교 : https://goodteacher.tistory.com/542  https://hyeonyeee.tistory.com/80
     *  computed : 리턴값 o, 동기 처리, 한 번 렌더링 됐을 때 update되면 될 때
     *  watch : 리턴값 x, 비동기 처리, 프로퍼티 변경시점에 action이 필요할 때(api call, router), 데이터의 실시간/빈번한 update가 필요할 때
    */

    const num = ref(4);

    const squareC = computed(() => {
      return num.value * num.value; //리턴된 값 사용. 템플릿에도 squareC가 사용됨
    })

    const squareW = ref(0); //단순한 ref

    watch(
      num, //첫 번째 파라미터 : 감시대상
      (cur, prev) => { // 두 번째 파라미터 : 감시 대상 변경시 동작할 callback으로 새로운 값(cur)과 기존 값(prev)이 파라미터로 전달됨
        console.log(`num 변경 ${prev} ==> ${cur}`);
        squareW.value = cur * cur; //callback 내부에서는 cur를 이용해서 squareW를 변경하고 있음
      },
      {
        immediate: true,
      }
    )

    return{
      cnt1,
      updateCnt1, 
      age, 
      can,
      num,
      squareC,
      squareW
    }
  }
}
</script>
<style scoped>
h1{
  font-size: 16px;
}
.is-error{
  background: #ff0000;
  color: #000;
}
</style>
