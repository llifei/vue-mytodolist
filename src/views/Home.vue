<template>
  <div>
    <header>
      <section>
        <label for="title">ToDoList</label>
        <input type="text" v-model="todo" v-on:keyup.enter="addTodo" placeholder="添加ToDo"/>
        <button type="button" @click="addTodo">添加</button>
      </section>
    </header>
    <section>
      <h2>
        待完成
        <span>{{todoLen}}</span>
      </h2>
      <ol class="demo-box">
        <template v-for="(item,index) in todoList" :key="index">
          <li v-if="!item.done">
            <input type="checkbox" @click="changeTodo(index,true)">
            <p>{{item.todo}}</p>
            <a @click="deleteTodo(index,false)">-</a>
          </li>
        </template>
      </ol>
      <h2>
        已完成
        <span>{{todoList.length-todoLen}}</span>
      </h2>
      <ul>
        <template  v-for="(item,index) in todoList" :key="index">
          <li draggable="true" v-if="item.done">
            <input type="checkbox" checked="checked" @click="changeTodo(index,false)">
            <p>{{item.todo}}</p>
            <a @click="deleteTodo(index,true)">-</a>
          </li>
        </template>
      </ul>
    </section>
    <footer>
      Copyright &copy; 2021  <a id="address" href="https://github.com/llifei/vue-mytodolist" target="_blank">github地址&nbsp;&nbsp;&nbsp;</a> 
      <a id="clear" @click="clearData">clear</a>
    </footer>
  </div>
</template>


<script>
import * as Utils from '@/utils/utils'
export default {
  name: 'Todolist',
  data(){
    return{
      todo: '',
      todoList: [],
      todoLen: 0
    }
  },
  methods:{
    addTodo(){
      if(this.todo === '')
        alert("输入不能为空！")
      else{
        let todoObj = {
          todo: this.todo,
          done: false
        }
        var tempList = Utils.getItem('todList')
        if(tempList){
          tempList.push(todoObj)
          Utils.setItem('todoList',tempList)
        }else{
          var tempData = []
          tempData.push(todoObj)
          Utils.setItem('todoList',tempData)
        }
        this.todoList.push(todoObj)
        this.todoLen++
        this.todo = ''
      }
    },
    deleteTodo(index,done){
      if(!done)
        this.todoLen--
      this.todoList.splice(index,1)
      Utils.setItem('todoList',this.todoList)
    },

    changeTodo(index,done){
      if(done){
        this.todoLen--
        this.todoList[index].done = true
        Utils.setItem('todoList',this.todoList)
      }else{
        this.todoLen++
        this.todoList[index].done = false
        Utils.setItem('tidoList',this.todoList)
      }
    },
    clearData(){
      localStorage.clear()
      this.todoList = []
      this.todoLen = 0
    },
    initTodo(){
      var todoArr = Utils.getItem('todoList')
      if(todoArr){
        for(let i = 0,len = todoArr.length; i < len; i++)
          if(todoArr[i].done === false)
            this.todoLen++
        this.todoList = todoArr
      }
    },
  },
  mounted(){
    this.initTodo()
  }
}
</script>


