<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
  </div>
</template>

<script>
// 会自动生成 new Vue({data:data})
export default {
  //等价于unction data () {return{}}
  // function data () {
  //   return {
  //     msg: 'Hello World'
  //   }
  // }

  data () {
    return {
      // note: changing this line won't causes changes
      // with hot-reload because the reloaded component
      // preserves its current state and we are modifying
      // its initial state.
      // 返回要渲染的数据
      msg: 'Hello World!',
      msg2: 'Hello Stephen'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: #42b983;
}
</style>
