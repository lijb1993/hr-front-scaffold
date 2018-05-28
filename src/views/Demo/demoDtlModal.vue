<template>
  <el-dialog title="仓库详情" :visible.sync="isShowDtlModal" @close="closeModal">
    <el-form :label-position="labelPosition" label-width="80px" :model="whse">
      <el-row :gutter="20">
        <el-col :span="12">
          <el-form-item label="仓库编号">
            <el-input v-model="whse.whseCode" :disabled="true"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="仓库名称">
            <el-input v-model="whse.whseName" :disabled="true"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="12">
          <el-form-item label="联系人">
            <el-input v-model="whse.ctcPer" :disabled="true"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="联系电话">
            <el-input v-model="whse.ctcTel" :disabled="true"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-form-item label="仓库描述">
        <el-input type="textarea" v-model="whse.whseDesc" :disabled="true"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="closeModal();">取 消</el-button>
      <!-- <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button> -->
    </div>
  </el-dialog>
</template>
<script type="text/ecmascript-6">
// import help from "../help.vue";
export default {
  // 组件传值
  props: ["dialogFormVisible", "whseId"],
  data() {
    return {
      isShowDtlModal: false,
      // lable位置
      labelPosition: "right",
      // 仓库数据对象
      whse: {
        whseName: "",
        whseCode: "",
        ctcPer: "",
        ctcTel: "",
        whseDesc: ""
      },
      gmt_passport_token:
        "f467b1cb61f746bbdca73eaba4a8f7147901211b983df74f8d5fd07c09ef77150c229c45c88ed0ad0f9d6ad5802563a6210ee658709c88d34bbf0fe2324028b520e1e55cd5462d7e"
    };
  },
  // 引用组件
  components: {
    // help: help,
  },
  //监听某一项data
  watch: {
    dialogFormVisible(val) {
      this.isShowDtlModal = val;
    },
    whseId(val) {
      console.log(val);
      this.getWhse();
    }
  },
  methods: {
    closeModal() {
      this.isShowDtlModal = false;
      this.$emit("closeDtlModal", this.isShowDtlModal);
    },
    getWhse() {
      this.axios
        .get("https://api.haorun.win/epsiv2/base/whse/getWhse", {
          headers: { gmt_passport_token: this.gmt_passport_token },
          params: { whseId: this.whseId }
        })
        .then(res => {
          console.log(res.data.data);
          this.whse = res.data.data;
        });
    }
  },
  // 初始化
  created() {}
};
</script>
<style lang="css" scoped>
</style>
