<template>
  <div>
    <el-form :inline="true" class="demo-form-inline">
    <el-form-item>
      <el-button type="primary" @click="addRole">添加角色</el-button>
    </el-form-item>
    </el-form>
    <el-table :data="list" border style="width: 100%">
      <el-table-column prop="roleId" label="ID" width="180" />
      <el-table-column prop="roleName" label="角色名" width="180" />
      <el-table-column prop="authority" label="操作">
        <template #default="scope">
          <el-button link size="small" @click="changeRole(scope.row)">
            修改权限
          </el-button>
        </template>
      </el-table-column>


    </el-table>
  </div>
</template>

<script setup lang='ts'>
import { ref, reactive, onMounted, toRefs } from 'vue'
import { getRoleList } from "../request/api"
import { InitData,ListInt } from '@/type/role'
import { ElMessage, ElMessageBox } from 'element-plus'
import {useRouter} from 'vue-router'
const data = reactive(new InitData())
const router=useRouter()
onMounted(() => {
  getRoleList().then(res => {
    //console.log(res)
    data.list = res.data.data
  })
})
//添加角色
const addRole = () => {
  ElMessageBox.prompt('请输入角色名称', '添加', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',

  })
    .then(({ value }) => {  //表示输入框中填写的值
      if (value) {//判断输入框中有值，就将对应的值添加到列表
        data.list.push({ roleId: data.list.length + 1, roleName: value, authority: [] })
      }
      ElMessage({
        type: 'success',
        message: `${value}角色添加成功`,
      })
    })
    .catch(() => {
      ElMessage({
        type: 'info',
        message: '取消操作',
      })
    })
 
}

// 修改权限
const changeRole=(row:ListInt)=>{
    router.push({
      name:"authority",
      params:{
        id:row.roleId,
        authority:row.authority,

      }
    })
}

const { list } = toRefs(data)
</script>
<style scoped></style>