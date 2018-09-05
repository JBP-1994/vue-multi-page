<template>
  <div class="">
    <input type="text" name="name" v-model="name">
    <input type="text" name="url" v-model="url">
    <button type="button" name="button" @click="add()">提交</button>
    <table>
      <tr v-for="item in list">
        <td>{{item.name}}</td>
        <td>{{item.url}}</td>
        <td>{{item._id}}</td>
        <td><button type="button" @click="del(item.name)" name="button">Delete</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'node',
  data () {
    return {
      name: 'test',
      url: 'www.baidu.com',
      list: []
    }
  },
  mounted () {
    let vue = this
    vue.get()
  },
  methods: {
    get: function () {
      let vue = this
      fetch('http://192.168.1.200:3000/list').then(response => response.json()).then(data => {
        vue.list = data
      }).catch(e => {
        console.log("Oops, error", e)
      })
    },
    del: function (name) {
      let vue = this
      fetch('http://192.168.1.200:3000/list?name='+name,{
        method: 'delete'
      }).then(response => response.json()).then(data => {
        vue.get()
      }).catch(e => {
        console.log("Oops, error", e)
      })
    },
    add: function () {
      let vue = this
      fetch('http://192.168.1.200:3000/list',{
        method: 'post',
        headers:{
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          name: this.name,
          url: this.url
        })
      }).then(response => response.json()).then(data => {
        vue.get()
      }).catch(e => {
        console.log("Oops, error", e)
      })
    }
  }
}
</script>


<style scoped>
table{
  width: 600px;
  margin: 0 auto;
}
</style>
