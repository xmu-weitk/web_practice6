<template>
  <el-dialog
    :title="title"
    :visible.sync="dialogPara.visible"
    width="400px"
  >
    <el-form :model="ruleForm" :rules="rules" ref="addForm" label-width="100px" class="good-form">
      <el-form-item label="图片" prop="url">
        <el-upload
          class="avatar-uploader"
          :action="uploadImgServer"
          accept="jpg,jpeg,png"
          :show-file-list="false"
          :before-upload="handleBeforeUpload"
          :on-success="handleUrlSuccess"
        >
          <img style="width: 200px; height: 100px; border: 1px solid #e9e9e9;" v-if="ruleForm.url" :src="ruleForm.url" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </el-form-item>
      <el-form-item label="跳转链接" prop="link">
        <el-input type="text" v-model="ruleForm.link"></el-input>
      </el-form-item>
      <el-form-item label="排序值" prop="sort">
        <el-input type="number" v-model="ruleForm.sort"></el-input>
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogPara.visible = false">取 消</el-button>
        <el-button type="primary" @click="submitForm">确 定</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<script>
import axios from '@/utils/axios'
import ElementUI from 'element-ui'
export default {
  name: 'SwiperDialog',
  components: {
   },
  props: {
    dialogPara: Object,
  },
  mounted() {
    console.log(this.dialogPara);
  },
  data() {
    return {
      ruleForm: {
        url: '',
        link: '',
        sort: ''
      },
      rules: {
        url: [
          { required: 'true', message: '图片不能为空', trigger: ['change'] }
        ],
        sort: [
          { required: 'true', message: '排序不能为空', trigger: ['change'] }
        ]
      },
      uploadImgServer:'',
      id: '',
      title: ''
    }
  },  
  methods: {
    handleBeforeUpload(file) {
      const sufix = file.name.split('.')[1] || ''
      if (!['jpg', 'jpeg', 'png'].includes(sufix)) {
        ElementUI.Message.error('请上传 jpg、jpeg、png 格式的图片')
        return false
      }
    },
    handleUrlSuccess(val) {
      this.ruleForm.url = val.data || ''
    },
    submitForm() {
      this.$refs["addForm"].validate((valid) => {
        if (valid) {
          if (this.dialogPara.type=='add'){
            axios.post('/api/carousels', {
                carouselUrl: this.ruleForm.url,
                redirectUrl: this.ruleForm.link,
                carouselRank: this.ruleForm.sort
              }).then(() => {
                ElementUI.Message.success('添加成功')
                this.dialogPara.visible = false
            })
          } else {
            axios.put('/api/carousels', {
                carouselId: this.id,
                carouselUrl: this.ruleForm.url,
                redirectUrl: this.ruleForm.link,
                carouselRank: this.ruleForm.sort
              }).then(() => {
                ElementUI.Message.success('修改成功')
                this.dialogPara.visible = false
            })
          }
        }
      })
    },
    formDefault(val) {
      this.ruleForm.url=val.carouselUrl
      this.ruleForm.link=val.redirectUrl
      this.ruleForm.sort=val.carouselRank
      this.id=val.carouselId
      if (this.dialogPara.type == 'add') {
        this.title = '添加轮播图'
      } else {
        this.title = '修改轮播图'
      }
    }
  }
}
</script>

<style scoped>
  .avatar-uploader {
    width: 100px;
    height: 100px;
    color: #ddd;
    font-size: 30px;
  }
  .avatar-uploader-icon {
    display: block;
    width: 100%;
    height: 100%;
    border: 1px solid #e9e9e9;
    padding: 32px 17px;
  }
</style>
