<template>
  <div>
    <ul>
      <li v-for="(todoItem, index) in propsdata" v-bind:key="todoItem.item" class="shadow">  <!-- v-bind:key는 VSCode에서만 뜨는거라. 원래는 없어도 되긴 함 -->
        <!-- 아래 v-bind:class를 통해 false, true에 따라 다르게 -->
        <i class="checkBtn fas fa-check" v-bind:class="{checkBtnCompleted: todoItem.completed}"
          v-on:click="toggleComplete(todoItem, index)"></i>
        <span v-bind:class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
        <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
          <i class="fas fa-trash-alt"></i>
        </span>
      </li>  
    </ul>
  </div>
</template>

<script>
export default {
  props: ['propsdata'],   // 상위 컴포넌트인 App.vue에서 내려줌
  methods: {
    // 삭제 기능
    removeTodo: function(todoItem, index){
      this.$emit('removeItem', todoItem, index);  // 상위 컴포넌트 App.vue로 올려줌
    },
    // 완료표시 기능
    toggleComplete: function(todoItem, index){
      this.$emit('toggleItem', todoItem, index);
    }
  },
}
</script>

<style scoped>
.removeBtn {
  margin-left: auto;
  color: #de4343;
}
.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}
.checkBtnCompleted {
  color: #c49f9f;
}
.textCompleted {
  text-decoration: line-through;
  color: #b3adad;
}
</style>