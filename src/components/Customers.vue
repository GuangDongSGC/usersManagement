<template>
  <div id="customers">
    <div class="container">
      <Alert v-if="test" :message="test"></Alert>
      <h1 class="page-header">用户管理系统</h1>
      <input type="text" class="form-control center-block" placeholder="输入用户名搜索" v-model="filterInput">
      <br>
      <table class="table table-striped">
        <thead>
          <tr class="text-center">
            <th>序号</th>
            <th>姓名</th>
            <th>年龄</th>
            <th>电话</th>
            <th>邮箱</th>
            <th>详情</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(customer,index) of filterBy(customers,filterInput)" :key="index">
            <td>{{customer.id}}</td>
            <td>{{customer.name}}</td>
            <td>{{customer.age}}</td>
            <td>{{customer.phone}}</td>
            <td>{{customer.email}}</td>
            <td><router-link :to="'customers/'+customer.id" class="btn btn-info">查看</router-link></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import Alert from './Alert'
export default {
  name: 'customers',
  components:{
    Alert
  },
  data () {
    return {
      customers:[],
      test:"",
      filterInput:""
    }
  },
  methods: {
    fetchCustomers(){
      this.$http.get("http://localhost:3000/users")
        .then(response=>{
          this.customers = response.body;
          console.log(this.customers);
        })
    },
    filterBy(customers,value){
      return customers.filter(function(customer,index){
        return customer.name.match(value);
      })
    }
  },
  created(){
    if(this.$route.query.test){
      this.test = this.$route.query.test;
      setTimeout(function(){
        $("#alert").css({"display":"none"});
      },3000)
    }
    this.fetchCustomers();
  },
  // updated(){
  //   this.fetchCustomers();
  // }
}
</script>

<style scoped>
table th, table td { 
  text-align: center;
  vertical-align: middle!important;
}
table tr:hover{
  background-color:coral;
  color: white;
}
input{
  width: 55vw !important;
}
</style>
