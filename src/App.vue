<!-- <template>
  <div id="app">
    <section class="todoapp">
      <header class="header">
        <h1>{{title}}</h1>
        <input class="new-todo" placeholder="What needs to be done?" autofocus :value="newTodo"
          v-model.trim="newTodo" @keyup.enter="addTodo">
      </header>
      This section should be hidden by default and shown when there are todos
      <section class="main">
        <input id="toggle-all" class="toggle-all" type="checkbox" v-model="isAll">
        <label for="toggle-all">Mark all as complete</label>
        <ul class="todo-list">
          <li v-for="(todo,index) in filteredTodos" :class="{completed:todo.completed,editing:todo == editedTodo}">
            <div class="view">
              <input class="toggle" type="checkbox" v-model="todo.completed">
              <label @dblclick="editTodo(todo)">{{todo.content}}</label>
              <button class="destroy" @click="removeTodo(index)"></button>
            </div>
            <input class="edit" value="Rule the web" v-model.trim="todo.content"
              v-focus="todo == editedTodo" @blur="doneEdit(todo,index)"
              @keyup.esc="cancleEdit(todo)"
              @keyup.enter="doneEdit(todo)"
              >
          </li>
        </ul>
      </section>
      This footer should hidden by default and shown when there are todos
      <footer class="footer" v-show="todos.length">
        This should be `0 items left` by default
        <span class="todo-count"><strong>{{remain}}</strong>{{todos.length>1?'items':'item'}} left</span>
        Remove this if you don't implement routing
        <ul class="filters">
          <li>
            <a :class="{selected:hashName=='all'}" href="#/">All</a>
          </li>
          <li>
            <a :class="{selected:hashName=='active'}" href="#/active">Active</a>
          </li>
          <li>
            <a :class="{selected:hashName=='completed'}" href="#/completed">Completed</a>
          </li>
        </ul>
        Hidden if no completed items are left ↓
        <button class="clear-completed" @click="clear">Clear completed</button>
      </footer>
    </section>
    <footer class="info">
      <p>Double-click to edit a todo</p>
      Remove the below line ↓
      <p>Template by <a href="http://sindresorhus.com">Sindre Sorhus</a></p>
      Change this out with your name and url ↓
      <p>Created by <a href="http://todomvc.com">you</a></p>
      <p>Part of <a href="http://todomvc.com">TodoMVC</a></p>
    </footer>
  </div>
</template>

<script>

//过滤器
  var filters = {
    all(todos){
      return todos
    },
    
    active(todos){
      return todos.filter((todo) => {
        return !todo.completed
      })
    },
    
    completed(todos){
      return todos.filter((todo) => {
        return todo.completed
      })
    },
  }
  
  function hashChange(){
  let hashName = location.hash.replace(/#\/?/,'')
  if(filters[hashName]){
    app.hashName = hashName
  }else{
    location.hash = "all"
    app.hashName = 'all'
  }
}
window.addEventListener('hashchange',hashChange);


export default {
  name: 'app',
  
    data:{
      msg:"hello world",
      title:'todos',
      newTodo:'',
      todos:[{
        content:'vue',
        completed:false
      },
      {
        content:'vuex',
        completed:false
      }],
      editedTodo:null,
      hashName:'all'
    },
    computed:{//计算属性
      remain(){//剩余多少
       return filters.active(this.todos).length
      },
      isAll:{
        get() {
          return this.remain === 0
        },
        set(value) {
          this.todos.forEach((todo) => {
            todo.completed = value
          })
        }
      },
      filteredTodos(){
        return filters[this.hashName](this.todos)
      }
      
    },
    methods:{
      addTodo(e){
        //console.log(e.target.value)
        if(!this.newTodo){
          return
        }
        this.todos.push({
          content:this.newTodo,
          completed:false
        })
        this.newTodo =''
      },
      removeTodo(index){
        this.todos.splice(index,1)
      },
      editTodo(todo){
        this.editCache = todo.content
        this.editedTodo = todo
      },
      doneEdit(todo,index){
        this.editedTodo = null
        if(!todo.content){
          this.removeTodo(index)
        }
      },
      cancleEdit(todo){
        this.editedTodo = null
        todo.content = this.editCache
      },
      clear(){
        this.todos = filters.active(this.todos)
      }
    },
    directives: {
      focus(el,value){
        if(value){
          el.focus()
        }
      }
    }
}



</script>

<style>
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
 -->
 <template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1 v-html="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished:item.isFinished}" v-on:click="toggleFinish(item)">
          {{item.label}}
      </li>
    </ul>
    <p >child tells me:{{childWords}}</p>
    <com-a msgfromfather="you die" v-on:child-tell-me-something="listenToMyBoy"></com-a>  
    <!--<router-view></router-view>-->
  </div>
</template>

<script>
import Store from './store.js'
import ComA from './components/comA'

export default {
  name: 'app',
  data () {
    return {
      title:"<span>?</span>this is a list",
      msg: 'Welcome come to Your Vue.js App',
      msg2:"my name is jason",
      items:Store.fetch(),
      newItem:"",
      childWords:''
    }
  },
  components:{ComA},
  watch:{
    items:{
      handler:function(items){
          Store.save(items);
      },
      deep:true
    }
  },
  methods:{
      toggleFinish:function(item){
        item.isFinished=!item.isFinished;
      },
      addNew:function(){
        this.items.push({
          label:this.newItem,
          isFinished:false
        })
        this.newItem='';
      },
      listenToMyBoy:function(msg){
          this.childWords = msg;
      }
    }
}
</script>

<style>
.finished{
  text-decoration:underline;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>