<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <!-- v-on: 하위에서 $emit으로 올라온 data (자식 -> 부모) -->
    <TodoInput v-on:addTodoItem="addTodo"></TodoInput> 
    <!-- v-bind: props를 사용해 TodoList.vue로 data를 내려줌 (부모 -> 자식) -->
    <TodoList :propsdata="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem"></TodoList>  
    <TodoFooter v-on:clearAllEvent="clearAllItems"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'


export default {
  data: function(){
    return {
      todoItems: []
    }
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter
  },
  // 생성되자마자 배열에 담아 View단에 v-for로 뿌려줌
  created: function(){

    if(localStorage.length > 0){
      for(var i=0; i<localStorage.length; i++){
        if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
          // JSON.stringify()로 넣어준 걸 JSON.parse()를 통해 다시 object로.
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));   // 로컬 스토리지에서 값을 빼와서 선언한 data에 담기
        }
      }
    }

  },
  // 싱글파일컴포넌트에 있던 코드들을 여기로 옮김
  methods: {
    // 추가 (TodoHeader.vue)
    addTodo: function(todoItem){   // TodoInput.vue에서 전송된 data가 인자 'todoItem'으로 넘어옴
      var obj = {completed: false, item: todoItem};
      // localStorage.setItem(this.newTodoItem, obj);  // [object Object]로 보여져서, data를 추적할 수 없음
      localStorage.setItem(todoItem, JSON.stringify(obj));  // javascript 객체 -> string 변환
      this.todoItems.push(obj);   // 배열 요소를 추가하는 javascript API
    },
    // 삭제 (TodoList.vue)
    removeOneItem: function(todoItem, index){
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);    // 배열 요소를 제거하는 javascript API
    },
    // 완료표시 기능 (TodoList.vue)
    toggleOneItem: function(todoItem, index){
      // props로 내린 data를 다시 $emit으로 올려서 바꾸는 패턴은 좋지 않다. => '안티패턴'이라고 부름.
        // 즉, "특정 컴포넌트의 data는 해당 컴포넌트 내에서만 바뀌는게 좋다."
        // ex) TodoList 컴포넌트의 data를 다른 컴포넌트에서 다루는 경우, 데이터 변경이 필요할 때 TodoList 컴포넌트에 요청해서 바꾸는 방식.
        // 여기에선 'todoItem'이란 매개변수 값을 직접 조작하는대신, index 매개변수를 통해 여기에 있는 data(todoItems 배열)에 직접 접근해 변경한다.
      // todoItem.completed = !todoItem.completed;   // false -> true
      this.todoItems[index].completed = !this.todoItems[index].completed;   // 그래서 이렇게 수정. 
      // 로컬스토리지 데이터 갱신 (로컬스토리지에 update기능이 없어, '삭제-추가'로 처리해야 함)
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    // 모두삭제 기능 (TodoFooter.vue)
    clearAllItems: function(){
      localStorage.clear();
      this.todoItems = [];    // 배열 clear
    }
  },

  
}
</script>


<style>
  body {
    text-align: center;
    background-color: #F6F6F6;
  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
  }
</style>
