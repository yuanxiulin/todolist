<template>
  <div style="height:100%">
    <el-container>
      <el-header>
        <h2>ToDoList</h2>
        <el-input placeholder="添加todo" v-model="todo" v-focus ></el-input>
        <el-button
          type="primary"
          @click="change"
          style="height:40px;letter-spacing:1px;"
          >添加todo</el-button
        >
      </el-header>

      <div style="display:flex;justify-content:space-between;margin:10px 20px;">
        <h3>正在进行</h3>
        <span style="font-size:16px;">{{ total }}</span>
      </div>
      <ul>
        <li v-for="(item, index) in list" :key="index" v-show="!item.done">
          <input
            class="el-checkbox__inner"
            type="checkbox"
            style="width:22px;height:22px;"
            @click="changeTodo(index,true)"
            v-model="item.checked"
          />
          <p style="line-height:32px;">{{ item.todo }}</p>
          <i class="el-icon-delete" @click="deletOne(index)"></i>
        </li>
      </ul>

      <div style="display:flex;justify-content:space-between;margin:10px 20px;">
        <h3>已经完成</h3>
        <span style="font-size:16px;">{{ list.length-total }}</span>
      </div>
      <ul>
        <li
          v-for="(item, index) in list"
          :key="index"
          class="classTrag"
          v-show="item.done"
        >
          <input
            class="el-checkbox__inner"
            type="checkbox"
            style="width:22px;height:22px;"
            v-model="item.checked"
            checked="checked"
            @click="changeTodo(index,false)"
          />
          <p style="line-height:32px;">{{ item.todo }}</p>
          <i class="el-icon-delete" @click="delet(index)"></i>
        </li>
      </ul>

       <el-footer>
         Copyright &copy; 2020 todolist.cn <span @click="clearData">clear</span>
       </el-footer>
    </el-container>
  </div>
</template>

<script>
import * as Index from '../utils/index.js'
export default {
  data() {
    return {
      list: [],
      todo: "",
      total: 0,
    };
  },
  //自动聚焦
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      },
    },
  },
  
  methods: {
    //添加
    change() {
      if (!this.todo) {
        return;
      }
      const todoobj = {
        todo: this.todo,
        done: false,
      };
      const templist=Index.getItem('list')
      if(templist){
        templist.unshift(todoobj);
        Index.setItem('list',templist)
      }else{
        let tempData=[]
        tempData.unshift(todoobj)
        Index.setItem('list',tempData)
      }
      this.list.unshift(todoobj);
      this.todo = "";
      this.total++;
    },
    //删除
    delet(index, done) {
      if (done) {
        this.total--;
      }
      this.list.splice(index, 1);
      Index.setItem('list',this.list);
    },
    //删除
    deletOne(index, done) {
      if (!done) {
        this.total--;
      }
      this.list.splice(index, 1);
      Index.setItem('list',this.list);
    },
    //计数
    changeTodo(index, done) {
      if (done) {
        this.total--;
        this.list[index].done=true;
        Index.setItem('list',this.list);
      } else {
        this.total++;
        this.list[index].done=false;
        Index.setItem('list',this.list);
      }
    },
    //存储
    initTodo(){
      let todoArr=Index.getItem('list');
      if(todoArr){
        for(let i=0;i<todoArr.length;i++){
            if(todoArr[i].done===false){
              console.log(todoArr[i].done)
              this.total++
            }
        }
        this.list=todoArr;
      }
    },
    //清除所有数据
    clearData(){
      localStorage.clear();
      this.list=[];
      this.total=0;
    }
  },
   mounted(){
    this.initTodo()
  }
 
};
</script>

<style lang="scss">
.is-vertical {
  background: #e9eef3;
  height: 100%;

  .el-header {
    background-color: #b3c0d1;
    color: #333;
    line-height: 60px;
    display: flex;
    text-align: -webkit-center;
    align-items: center;
    .el-input__inner {
      margin: 0 20px;
      width: 60%;
      outline: none !important;
      padding-left:10px;
    }
  }

  li {
    height: 32px;
    background: pink;
    margin: 10px 20px;
    text-align: left;
    border-left: 5px solid #629a9c;
    border-radius: 3px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #FFF;
    .el-checkbox__inner {
      width: 22px;
      height: 22px;
      margin-left: 10px;
    }
    .el-checkbox__inner::after {
      left: 8px !important;
      top: 5px !important;
    }
    .el-icon-delete {
      font-size: 18px;
      margin-right: 10px;
    }
    p {
      font-size: 18px;
      
    }
  }

  .classTrag {
    border-left: 5px solid #629a9c;
    opacity: 0.7;
    background: #999;
  }
  .el-footer{
    text-align: center;
    font-size:18px;
    color:#999;
    font-weight: bold;
    margin-top:20px;
    span{
      color:#629a9c;
    }
    
  }
}
</style>
