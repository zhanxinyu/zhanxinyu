<template>
  <div>
    <el-table
      :data="roleList"
      style="width: 100%; margin-bottom: 20px"
      row-key="id"
      border
      default-expand-all
      :tree-props="{ children: 'children', hasChildren: 'hasChildren' }"
    >
      <el-table-column prop="id" label="角色编号" sortable width="180">
      </el-table-column>
      <el-table-column prop="rolename" label="角色名称" sortable width="180">
      </el-table-column>
      <el-table-column prop="stayus" label="状态">
          <template slot-scope="scope">
          <el-button type="primary" v-if="scope.row.status == 1"
            >启用</el-button
          >
          <el-button type="info" v-else>禁用</el-button>
        </template>
      </el-table-column>
      <el-table-column prop="address" label="操作">
        <template slot-scope="scope">
           <el-button type="primary" @click ='edit(scope.row.id)'>编辑</el-button>
          <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
          </template>
    </el-table-column>
    </el-table>
  </div>
</template>
<script>
import {mapActions,mapGetters} from "vuex"
import { reqRoleDel } from "../../../util/request";
export default {
  computed:{
    ...mapGetters({
      roleList:"role/roleList"
    })
  },
  components: {},
  data() {
    return {
         
    };
  },
  methods: {
    ...mapActions({
      reqMenList: "menu/reqMenList",
      reqMenRoleList: "role/reqMenRoleList",
    }),
    del(id) {
      this.$confirm("是否删除?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          // 确认删除
          reqRoleDel({ id: id }).then((res) => {
            if (res.data.code == 200) {
              this.$message({
                type: "success",
                message: "删除成功!",
              });
              this.reqMenRoleList();
            }
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
    edit(id){
      this.$emit('edit',id)
    }
  },
  mounted() {
    this.reqMenRoleList()
    this.reqMenList()
  },
};
</script>
<style scoped>
</style>