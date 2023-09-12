


<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" @keyup.enter="addTodo">
    <!-- <button v-on:click="addTodo">add</button> -->
    <span class="addContainer"> 
      <i class="fas fa-plus" @click="addTodo"></i>
    </span>

    <Modal v-if="showModal" @close="showModal = false">
      <!--
        you can use custom content here to overwrite
        default content
      -->
      <h3 slot="header">경고 <i class="closeModalBtn far fa-times-circle" @click="showModal = false"></i></h3>
      <div slot="body">뭔갈 입력하셔야 합니다</div>
      <div slot="footer"></div>
    </Modal>
  </div>
</template>

<script>
import Modal from './common/Modal.vue';

export default {
  data: function(){
    return {
      newTodoItem: "",
      showModal: false
    }
  },
  components: {
    Modal: Modal
  },
  methods: {
    addTodo: function(){
      if(this.newTodoItem !== ''){
        this.$emit('addTodoItem', this.newTodoItem);   // App.vue로 전송. $emit('이벤트 명', 보낼 data)
        this.clearInput();
      } else {
        this.showModal = true;
      }
     
    },
    // input 박스 초기화
    clearInput: function(){
      this.newTodoItem = ''; 
    }
  }
  
}
</script>

<style scoped>
  input:focus {
    outline: none;
  }
  .inputBox {
    background-color: white;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
  }
  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }
  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478FB, #8763FB);
    display: block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
  }
  .addBtn {
    color: white;
    vertical-align: middle;
  }
  .closeModalBtn {
    color: #42B983;
  }

</style>


