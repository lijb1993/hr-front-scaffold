<template>
  <div>
    <el-row :gutter="20">
      <el-col :span="8">
        <el-input placeholder="请输入内容" prefix-icon="el-icon-search" v-model="query">
        </el-input>
      </el-col>
      <el-col :span="6" style="text-align:left">
        <el-button v-on:click="search();" type="primary" icon="el-icon-search">搜索</el-button>
      </el-col>
      <el-col :span="10" style="text-align:right">
        <el-button v-on:click="add();" type="primary" icon="el-icon-plus">新增</el-button>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
        <el-table class="table" ref="multipleTable" :data="whse.data.content" :stripe="true" style="width: 100%" @selection-change="handleSelectionChange">
          <el-table-column width="55" type="index">
            <template slot-scope="scope" style="text-align:center">
              <el-checkbox v-model="checked[whse.data.content[scope.$index].whseId]" @change="selectRow(scope.$index,scope.row.whseId)"></el-checkbox>
            </template>
          </el-table-column>
          <el-table-column property="whseName" label="仓库名称" width="150">
          </el-table-column>
          <el-table-column property="ctcPer" label="联系人" width="150">
          </el-table-column>
          <el-table-column property="ctcTel" label="联系电话" width="150">
          </el-table-column>
          <el-table-column property="whseAddr" label="仓库地址">
          </el-table-column>
          <el-table-column fixed="right" label="操作" width="80" align="center">
            <template slot-scope="scope" style="text-align:center">
              <el-button @click="deleteWhse(scope.row)" type="danger" size="small">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col style="text-align:right">
        <el-pagination background class="table" layout="prev, pager, next" :page-size='whse.data.pageable.size' :total='whse.data.total' @current-change='changePage'>
        </el-pagination>
      </el-col>
    </el-row>
    <demoDtlModal v-bind:dialogFormVisible="this.isShowDtlModal" v-bind:whseId="this.whseId" @closeDtlModal="closeDtlModal"></demoDtlModal>
    <demoAddModal v-bind:showAddModal="this.isShowAddModal" @closeAddModal="closeAddModal"></demoAddModal>
  </div>
</template>
<script type="text/ecmascript-6">
import demoDtlModal from "./demoDtlModal.vue";
import demoAddModal from "./demoAddModal.vue";
export default {
  // 组件传值
  props: ["dialogFormVisible", "showAddModal"],
  data() {
    return {
      query: "",
      stripe: true,
      whse: {
        resultCode: null,
        message: "",
        data: {
          total: null,
          pageable: {
            size: null,
            page: null,
            sort: null
          }
        },
        content: []
      },
      gmt_passport_token:
        "f467b1cb61f746bbdca73eaba4a8f7147901211b983df74f8d5fd07c09ef77150c229c45c88ed0ad0f9d6ad5802563a6210ee658709c88d34bbf0fe2324028b520e1e55cd5462d7e",
      currentRow: null,
      currentPage: 1,
      isShowDtlModal: this.dialogFormVisible,
      isShowAddModal: this.showAddModal,
      whseId: "",
      multipleSelection: [],
      checked: []
    };
  },
  // 引用组件
  components: {
    demoDtlModal: demoDtlModal,
    demoAddModal: demoAddModal
  },
  methods: {
    // 查询按钮
    search() {
      console.log(this.query);
      this.getdata({ page: this.currentPage, size: 10, qk: this.query });
    },
    // 选择行
    handleSelectionChange(currentRow) {
      console.log(currentRow[currentRow.length - 1]);
      // this.$refs.multipleTable.toggleRowSelection(currentRow[currentRow.length - 1]);
    },
    // 接口
    getdata(option) {
      this.axios
        .get("https://api.haorun.win/epsiv2/base/whse/findWhsePage", {
          headers: { gmt_passport_token: this.gmt_passport_token },
          params: option
        })
        .then(res => {
          console.log(res.data);
          this.whse = res.data;
        });
    },
    // 换页
    changePage(pager) {
      console.log(pager);
      this.currentPage = pager;
      this.getdata({ page: this.currentPage, size: 10, qk: this.query });
    },
    //关闭详情弹窗
    closeDtlModal: function(data) {
      this.isShowDtlModal = data;
      console.log(data);
    },
    //关闭新增弹窗
    closeAddModal: function(data) {
      this.isShowAddModal = data;
      console.log(data);
    },
    // 新增
    add() {
      this.isShowAddModal = true;
    },
    selectRow(data, row) {
      // console.log(data);
      // console.log(row);
      console.log(this.checked);
      // console.log(this.checked[row]);
      // this.whse.data.content.forEach(function(item) {
      //   if (row === item.whseId) {
      //     console.log(item);
      //   } else {
      //     this.checked[item.whseId] = false;
      //   }
      // });
    }
  },
  // 初始化
  created() {
    this.getdata({ page: 1, size: 10 });
  }
};
</script>
<style lang="css" scoped>
</style>
