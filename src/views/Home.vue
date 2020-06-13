<template>
  <div>
    <header>
      <section>
        <label for="title">ToDoList</label>
        <!-- 输入框，用v-model绑定todo，监听enter键 -->
        <input type="text" v-model="todo"  @keyup.enter="addTodo" placeholder="添加ToDo" />
      </section>
    </header>
    <!-- 进行及完成模块 -->
    <section>
      <h2>正在进行
        <!-- 进行中任务的个数 -->
        <span>{{todoLen}}</span>
      </h2>
      <!-- 使用v-for将所添加的事项循环显示，并使用v-if条件渲染只显示的内容 -->
      <ol class="demo-box" >
        <template v-for="(item, index) in todoList" >
          <li v-if="item.done === false" item.todo=item.todo :key="index">
            <input type="checkbox" @change="changeTodo(index,true)">
            <p>{{item.todo}}</p>
            <a @click="deleteTodo(index,true)">-</a>
          </li>
        </template>
      </ol>
      <h2>已经完成
        <span>{{todoList.length - todoLen}}</span>
      </h2>
      <!-- 使用v-for将完成的事项循环显示，并使用v-if条件渲染只显示的内容 -->
      <ul >
        <template v-for="(item, index) in todoList" >
          <li v-if="item.done === true" item.todo=item.todo :key="index">
            <input type="checkbox" @change="changeTodo(index,false)" checked="checked">
            <p>{{item.todo}}</p>
            <a @click="deleteTodo(index,false)">-</a>
          </li>
        </template>
      </ul>
    </section>
    <footer>
      Copyright &copy; 2020 todolist.cn
      <a @click="clearData()">clear</a>
    </footer>
  </div>
</template>

<script>
import * as Utils from "@/utils/localStorage";
export default {
  name: "Todolist",
  data() {
    return {
      todo: "",
      todoList: [],
      todoLen: 0
    };
  },
  methods: {
    // 初始化todo列表
    initTodo() {
      // 获取localStorage中的todoList数据
      var todoArr = Utils.getItem("todoList");
      if (todoArr) {
        for (let i = 0, len = todoArr.length; i < len; i++) {
          if (todoArr[i].done === false) {
            this.todoLen++;
          }
        }
        // 将获取的数据赋给todolist
        this.todoList = todoArr;
      }
    },
    // 增加todo项
    addTodo() {
      let todoObj = {
        todo: this.todo,
        done: false
      };
      var tempList = Utils.getItem("todoList");
      if (tempList) {
        tempList.push(todoObj);
        Utils.setItem("todoList", tempList);
      } else {
        var tempData = [];
        tempData.push(todoObj);
        Utils.setItem("todoList", tempData);
      }
      this.todoList.push(todoObj);
      this.todoLen++;
      this.todo = "";
    },
    // 删除todolist项
    deleteTodo(index, done) {
      if (done) {
        this.todoLen--;
      }
      this.todoList.splice(index, 1);
      Utils.setItem("todoList", this.todoList);
    },
    changeTodo(index, done) {
      if (done) {
        this.todoLen--;
        this.todoList[index].done = true;
        Utils.setItem("todoList", this.todoList);
      } else {
        this.todoLen++;
        this.todoList[index].done = false;
        Utils.setItem("todoList", this.todoList);
      }
    },

    // 清除所有数据
    clearData() {
      localStorage.clear();
      this.todoList = [];
      this.todoLen = 0;
    }
  },
  // 刷新页面立即渲染localStorege数据
  mounted() {
    this.initTodo();
  }
};
</script>
