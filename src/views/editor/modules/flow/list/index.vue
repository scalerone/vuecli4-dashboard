<template>
  <div class="flow-list">
    <el-row>
      <el-col :span="24">
        <div class="grid-content">
          <span>流程筛选</span>
          <el-button
            type="primary"
            icon="el-icon-edit"
            @click="handleCreate"
          >{{ $t('flowlist.btnSave') }}</el-button>
        </div>
      </el-col>
    </el-row>

    <el-divider class="divider"></el-divider>
    <el-form :inline="true" :model="formInline" class="form-inline">
      <el-form-item label="项目">
        <el-input v-model="formInline.user" placeholder="项目"></el-input>
      </el-form-item>
      <el-form-item label="手机号">
        <el-input v-model="formInline.region" placeholder="手机号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
    <el-row>
      <el-col :span="24">
        <div class="grid-content">
          <span>流程列表</span>
        </div>
      </el-col>
    </el-row>
    <el-divider></el-divider>

    <!-- <el-divider content-position="left">用户列表</el-divider> -->
    <el-table :data="tableData" style="width: 100%;">
      <el-table-column prop="id" label="ID" width="180"></el-table-column>
      <el-table-column prop="flowName" label="name"></el-table-column>
      <el-table-column label="创建时间" width="250">
        <template slot-scope="scope">
          <i class="el-icon-time"></i>
          <span style="margin-left: 10px">{{ scope.row.date }}</span>
        </template>
      </el-table-column>

      <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="handleEdit(scope)">编辑</el-button>
          <el-button type="text" size="small" @click="del(scope)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage4"
      :page-sizes="[100, 200, 300, 400]"
      :page-size="100"
      layout="total, sizes, prev, pager, next, jumper"
      :total="tableData.length"
      style="    text-align: right"
    ></el-pagination>
  </div>
</template>

<script>
// import _ from "lodash";
import { getflowlist, delFlow } from "@/api/flow";
import { Base64 } from "js-base64";
export default {
  watch: {
    // flowData(val) {
    // }
  },
  props: {
    // data: {
    //   type: Object,
    //   default: false
    // }
  },
  components: {},
  data: function() {
    return {
      addDialogFormVisible: false,
      formType: "add",
      formInline: {
        user: "",
        region: ""
      },
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄"
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄"
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄"
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄"
        }
      ],
      currentPage1: 5,
      currentPage2: 5,
      currentPage3: 5,
      currentPage4: 4,
      currentPage: 1,
      pageSize: 5
    };
  },
  computed: {
    //...mapState([""])
  },
  mounted() {
    this.initData({ pageSize: this.pageSize, currentPage: this.currentPage });
  },
  beforeCreate() {},
  created() {},
  beforeMount() {},
  beforeUpdate() {},
  updated() {},
  beforeDestroy() {},
  destroyed: function() {},
  methods: {
    //...mapActions([""]),
    onSubmit() {
      console.log("submit!");
    },
    resetForm() {},
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
    handleCreate() {
      this.$router.push({ path: "/editor/jspluimbchart" });
    },
    handleEdit({ row }) {
      this.$router.push({
        path: "/editor/jspluimbchart",
        query: { row: encodeURIComponent(Base64.encode(JSON.stringify(row))) }
      });
    },
    del({ $index, row }) {
      this.$confirm("Confirm to remove the role?", "Warning", {
        confirmButtonText: "Confirm",
        cancelButtonText: "Cancel",
        type: "warning"
      })
        .then(async () => {
          await delFlow(row.id).then(res => {
            if (res.data.code == 20000) {
              this.tableData.splice($index, 1);
              this.$message({
                type: "success",
                message: "Delete succed!"
              });
            }
          });
        })
        .catch(err => {
          console.error(err);
        });
    },
    cancel(val) {
      this.addDialogFormVisible = false;
      this.formType = val;
    },
    initData(val) {
      getflowlist(val).then(res => {
        this.tableData = res.data.data;
      });
    }
  }
};
</script>

<style lang="scss">
.flow-list {
  position: absolute;
  padding: 15px;
  width: 100%;
  background: #ffffff;
  .form-inline {
    padding: 10px 0 20px 0;
    // display: flex;
    // justify-content: space-between;
  }

  .el-divider--horizontal {
    margin-top: 10px !important;
  }

  .grid-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
}
</style>
