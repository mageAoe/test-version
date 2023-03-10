<template>
  <div class="dialog">
    <el-dialog
      title="收货地址"
      :visible="dialogFormVisible"
      :close-on-click-modal="false"
      @close="close"
    >
      <el-form :model="form" ref="ruleForm">
        <el-form-item
          label="活动名称"
          :label-width="formLabelWidth"
          prop="name"
        >
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item
          label="活动区域"
          :label-width="formLabelWidth"
          prop="address"
        >
          <el-select v-model="form.address" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="close">取 消</el-button>
        <el-button type="primary" @click="add">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    dialogFormVisible: Boolean,
  },
  data: () => ({
    form: {
      name: "",
      address: "",
    },
    formLabelWidth: "120px",
  }),
  methods: {
    close() {
      this.$emit("close", false);
      this.$nextTick(() => {
        this.resetForm();
      });
    },
    add() {
      this.$emit("add", this.form);
      this.close();
    },
    resetForm() {
      this.$refs.ruleForm.resetFields();
    },
    getFrom(index, value) {
      console.log(index, value);
      this.form = { ...value };
    },
  },
};
</script>

<style scoped></style>
