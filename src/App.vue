<template>
  <div id="app">
    <h1>{{title}}</h1>
    <h2 v-html="title2"></h2>
    <!-- v-model双向绑定 -->
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
        {{item.label}}
      </li>
    </ul>
    <p>chlid tells me: {{ childWords }}</p>
    <!-- 自定义事件child-msg 用于监听emit -->
    <comp1 msgfromfather='hello son' v-on:child-msg='listenToMyChild'></comp1>
  </div>
</template>

<script>
// import Hello from './components/Hello'
// 引入JS
import Store from './localstorage'
import Comp1 from './components/comp1'

//等价于models.export
export default {
  data: function(){
    return {
      title: 'this is a todo list',
      title2: '<span>by </span>Stephen',
      newItem: '',
      items: Store.fetch(),
      childWords: ''
    }
  },
  components: {
    Comp1
  },
  // 观察器 
  watch: {
    //观察items的变化，自动触发更新
    items: {
      handler: function(items){
        //items变化触发保存到localstorage
        Store.save(items)
      },
      deep: true // 深层复制
    }
  },
  // 使用$dispatch需要使用events监听
  events: {
    'child-msg': function (msg) {
      // 事件回调内的 `this` 自动绑定到注册它的实例上
      this.childWords = msg
    }
  },
  //方法放在methods
  methods: {
    toggleFinish: function(item) {
      // console.log(item)
      item.isFinished = !item.isFinished
    }, // es6方法写法
    addNew() {
      // console.log(this.newItem)
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
      this.$broadcast('onAddnew', this.items) //向子级广播
    },
    //监听子级传来的参数msg
    listenToMyChild: function(msg){
      console.log(msg)
      this.childWords = msg
    }
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

#app {
  color: #2c3e50;
  margin-top: -100px;
  max-width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
}

#app a {
  color: #42b983;
  text-decoration: none;
}

.finished{
  color: #42b983;
}

.logo {
  width: 100px;
  height: 100px
}
</style>
