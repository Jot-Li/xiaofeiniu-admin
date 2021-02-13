<template>
  <div class="main">
    欢迎回来：{{$store.state.adminName}}
    <h1>This is main page</h1>
    <div>左侧的导航菜单</div>
    <div>上方的提示栏</div>
    <div>
      <h3>右下角主体部分</h3>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  beforeCreate(){
    //组件创建之前先要检查是否已经登录为管理员
    if(!this.$store.state.adminName){
      this.$router.push('/login');//未登录则跳转
    }
  }
}
</script>
