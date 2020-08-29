<template>

  <el-card class="box-card">
  <div  class="text item">
    <h2>欢迎登录Skr后台管理系统!</h2>
    <p>UserName：<i>{{list.username}}</i></p>
    <p>Password：<i>{{list.password}}</i></p>
    <canvas id="canvas">111</canvas>
  </div>
</el-card>
  
</template>
<script>
export default {
  data(){
    return{
      list:{username:'admin',password:'123456'}
    }
  }
}
</script>
<style scoped>
.center{
  text-align: center;
}
.center h3{
  color: rgb(23, 228, 40);
}
.text {
  font-size: 14px;
}

.item {
  padding: 18px;
}

.box-card {
  width: 480px;
  margin: 0 auto;
}
.item p{
  margin-left: 100px;
}
h2{
  margin-left: 50px;
}
i{
  font-style: normal;
  color: green;
}
</style>