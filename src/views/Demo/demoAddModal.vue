<template>
  <el-dialog title="新增仓库" :visible.sync="isShowAddModal" @close="closeModal">
    <el-form ref='addWhse' :label-position="labelPosition" label-width="80px" :model="whse" :rules="rules">
      <el-row :gutter="20">
        <el-col :span="12">
          <el-form-item label="仓库编号">
            <el-input v-model="whse.whseCode"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="仓库名称" prop="whseName">
            <el-input v-model="whse.whseName"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="12">
          <el-form-item label="联系人">
            <el-input v-model="whse.ctcPer"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="联系电话">
            <el-input v-model="whse.ctcTel"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-form-item label="仓库描述">
        <el-input type="textarea" v-model="whse.whseDesc"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="closeModal();">取 消</el-button>
      <el-button type="primary" @click="saveWhse('addWhse')">保存</el-button>
    </div>
  </el-dialog>
</template>
<script type="text/ecmascript-6">
// import help from "../help.vue";
export default {
  // 组件传值
  props: ["showAddModal"],
  data() {
    return {
      // 是否显示弹窗
      isShowAddModal: false,
      // 仓库数据对象
      whse: {
        whseName: "",
        whseCode: "",
        ctcPer: "",
        ctcTel: "",
        whseDesc: ""
      },
      // 表单验证规则
      rules: {
        whseName: [
          { required: true, message: "请输入仓库名称", trigger: "blur" }
        ]
      },
      // lable位置
      labelPosition: "right",
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
    showAddModal(val) {
      this.isShowAddModal = val;
    }
  },
  methods: {
    closeModal() {
      this.isShowAddModal = false;
      this.whse = {};
      this.$emit("closeAddModal", this.isShowAddModal);
    },
    // 保存
    saveWhse(formName) {
      console.log(this.whse);
      // 验证表单
      this.$refs[formName].validate(valid => {
        if (valid) {
          console.log(this.whse);
          // 调用保存接口
          // this.axios({
          //   method: "post",
          //   headers: { gmt_passport_token: this.gmt_passport_token },
          //   url: "https://api.haorun.win/epsiv2/base/whse/addWhse",
          //   data: {
          //     whseBody: this.whse
          //   }
          // });
          this.$message({
            message: "保存成功",
            type: "success"
          });
          this.closeModal();
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  },

  // 初始化
  created() {}
};
</script>
<style lang="css" scoped>
</style>
