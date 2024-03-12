<template>
  <van-card
      v-for="(user,index) in userList"
      :key="index"
      :desc="user.profile"
      :title="`${user.userName}`"
      :thumb="user.avatar"
  >
  </van-card>
  <van-empty v-if="!userList || userList.length < 1" description="搜索结果为空" />
</template>
<script setup lang="ts">
import {useRoute} from "vue-router";
import {onMounted, ref} from "vue";
import myAxios from "../plugins/myAxios";
import qs from "qs";

const route = useRoute();
const {tags} = route.query;

const mockUser = ref({
  id: 1,
  userName: 'xin',
  userAccount: 'xin',
  profile: 'xin的个人简介，全栈工程师，加油加油加油！！！！！',
  gender: '男',
  phone: '234234',
  email: '1154379178@qq.com',
  planetCode: '1017',
  avatar: 'https://img1.baidu.com/it/u=1645832847,2375824523&fm=253&fmt=auto&app=138&f=JPEG?w=480&h=480',
  createTime: new Date().toDateString()
})

const userList = ref([]);//存放用户列表

onMounted ( async ()=>{     //异步调用
  const userListData = await myAxios.get('/user/search/tags',{
    params: {
      tagNameList: tags
    },
    paramsSerializer: params => {
      return qs.stringify(params, {indices: false})
    },
  })
      .then(function (response){
        console.log('/user/search/tags succeed',response);
        // Toast.show('请求成功!')
        return response?.data;  //返回数据  ?.可选链操作符，避免数据为null或undefined时报错
      })
      .catch(function (error){
        console.error('/user/search/tags error',error);
        // Toast.show('请求失败!')
      })
  // console.log(userListData)
  if(userListData){
    // userListData.forEach(user => {
    //   if(user.tags){
    //     user.tags = JSON.parse(user.tags)
    //   }
    // })
    userList.value = userListData
    // console.log(userList)
  }
})
</script>

<style scoped>

</style>