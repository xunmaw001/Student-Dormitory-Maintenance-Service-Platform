<template>
  <div>
    <el-form
      class="detail-form-content"
      ref="ruleForm"
      :model="ruleForm"
      label-width="80px"
    >  
     <el-row>
                    <el-col :span="12">
           <el-form-item v-if="flag=='weixiuyuan'"  label='维修员姓名' prop="weixiuyuanName">
               <el-input v-model="ruleForm.weixiuyuanName"  placeholder='维修员姓名' clearable></el-input>
           </el-form-item>
         </el-col>

         <el-col :span="12">
           <el-form-item v-if="flag=='weixiuyuan'"  label='工作年限' prop="weixiuyuanNianxian">
               <el-input v-model="ruleForm.weixiuyuanNianxian"  placeholder='工作年限' clearable></el-input>
           </el-form-item>
         </el-col>

         <el-col :span="12">
           <el-form-item v-if="flag=='weixiuyuan'"  label='联系方式' prop="weixiuyuanPhone">
               <el-input v-model="ruleForm.weixiuyuanPhone"  placeholder='联系方式' clearable></el-input>
           </el-form-item>
         </el-col>

         <el-col :span="12">
             <el-form-item v-if="flag=='weixiuyuan'" label='照片' prop="weixiuyuanPhoto">
                 <file-upload
                         tip="点击上传照片"
                         action="file/upload"
                         :limit="3"
                         :multiple="true"
                         :fileUrls="ruleForm.weixiuyuanPhoto?ruleForm.weixiuyuanPhoto:''"
                         @change="weixiuyuanPhotoUploadChange"
                 ></file-upload>
             </el-form-item>
         </el-col>
         <el-col :span="24">
             <el-form-item v-if="flag=='weixiuyuan'"  label="维修人员介绍" prop="weixiuyuanContent">
                 <editor
                         style="min-width: 200px; max-width: 600px;"
                         v-model="ruleForm.weixiuyuanContent"
                         class="editor"
                         action="file/upload">
                 </editor>
             </el-form-item>
         </el-col>
         <el-col :span="12">
           <el-form-item v-if="flag=='xuesheng'"  label='学生姓名' prop="xueshengName">
               <el-input v-model="ruleForm.xueshengName"  placeholder='学生姓名' clearable></el-input>
           </el-form-item>
         </el-col>

         <el-col :span="12">
           <el-form-item v-if="flag=='xuesheng'"  label='身份证号' prop="xueshengIdNumber">
               <el-input v-model="ruleForm.xueshengIdNumber"  placeholder='身份证号' clearable></el-input>
           </el-form-item>
         </el-col>

         <el-col :span="12">
           <el-form-item v-if="flag=='xuesheng'"  label='手机号' prop="xueshengPhone">
               <el-input v-model="ruleForm.xueshengPhone"  placeholder='手机号' clearable></el-input>
           </el-form-item>
         </el-col>

         <el-col :span="12">
             <el-form-item v-if="flag=='xuesheng'" label='照片' prop="xueshengPhoto">
                 <file-upload
                         tip="点击上传照片"
                         action="file/upload"
                         :limit="3"
                         :multiple="true"
                         :fileUrls="ruleForm.xueshengPhoto?ruleForm.xueshengPhoto:''"
                         @change="xueshengPhotoUploadChange"
                 ></file-upload>
             </el-form-item>
         </el-col>
         <el-form-item v-if="flag=='users'" label="用户名" prop="username">
             <el-input v-model="ruleForm.username"
                       placeholder="用户名"></el-input>
         </el-form-item>
         <el-col :span="12">
             <el-form-item v-if="flag!='users'"  label="性别" prop="sexTypes">
                 <el-select v-model="ruleForm.sexTypes" placeholder="请选择性别">
                     <el-option
                             v-for="(item,index) in sexTypesOptions"
                             v-bind:key="item.codeIndex"
                             :label="item.indexName"
                             :value="item.codeIndex">
                     </el-option>
                 </el-select>
             </el-form-item>
         </el-col>
         <el-col :span="24">
             <el-form-item>
                 <el-button type="primary" @click="onUpdateHandler">修 改</el-button>
             </el-form-item>
         </el-col>
     </el-row>
    </el-form>
  </div>
</template>
<script>
// 数字，邮件，手机，url，身份证校验
import { isNumber,isIntNumer,isEmail,isMobile,isPhone,isURL,checkIdCard } from "@/utils/validate";

export default {
  data() {
    return {
      ruleForm: {},
      flag: '',
      usersFlag: false,
      sexTypesOptions : [],
    };
  },
  mounted() {
    //获取当前登录用户的信息
    var table = this.$storage.get("sessionTable");
    this.sessionTable = this.$storage.get("sessionTable");
    this.role = this.$storage.get("role");
    if (this.role != "管理员"){
    }

    this.flag = table;
    this.$http({
      url: `${this.$storage.get("sessionTable")}/session`,
      method: "get"
    }).then(({ data }) => {
      if (data && data.code === 0) {
        this.ruleForm = data.data;
      } else {
        this.$message.error(data.msg);
      }
    });
      this.$http({
          url: `dictionary/page?page=1&limit=100&sort=&order=&dicCode=sex_types`,
          method: "get"
      }).then(({ data }) => {
          if (data && data.code === 0) {
          this.sexTypesOptions = data.data.list;
      } else {
          this.$message.error(data.msg);
      }
  });
  },
  methods: {
    weixiuyuanPhotoUploadChange(fileUrls) {
        this.ruleForm.weixiuyuanPhoto = fileUrls;
    },
    xueshengPhotoUploadChange(fileUrls) {
        this.ruleForm.xueshengPhoto = fileUrls;
    },

    onUpdateHandler() {
                         if((!this.ruleForm.weixiuyuanName)&& 'weixiuyuan'==this.flag){
                             this.$message.error('维修员姓名不能为空');
                             return
                         }

                         if((!this.ruleForm.weixiuyuanNianxian)&& 'weixiuyuan'==this.flag){
                             this.$message.error('工作年限不能为空');
                             return
                         }

                         if((!this.ruleForm.weixiuyuanPhone)&& 'weixiuyuan'==this.flag){
                             this.$message.error('联系方式不能为空');
                             return
                         }

                         if((!this.ruleForm.weixiuyuanPhoto)&& 'weixiuyuan'==this.flag){
                             this.$message.error('照片不能为空');
                             return
                         }

                         if((!this.ruleForm.weixiuyuanContent)&& 'weixiuyuan'==this.flag){
                             this.$message.error('维修人员介绍不能为空');
                             return
                         }

                         if((!this.ruleForm.xueshengName)&& 'xuesheng'==this.flag){
                             this.$message.error('学生姓名不能为空');
                             return
                         }

                         if((!this.ruleForm.xueshengIdNumber)&& 'xuesheng'==this.flag){
                             this.$message.error('身份证号不能为空');
                             return
                         }

                             if( 'xuesheng' ==this.flag && this.ruleForm.xueshengPhone&&(!isMobile(this.ruleForm.xueshengPhone))){
                                 this.$message.error(`手机应输入手机格式`);
                                 return
                             }
                         if((!this.ruleForm.xueshengPhoto)&& 'xuesheng'==this.flag){
                             this.$message.error('照片不能为空');
                             return
                         }

        if((!this.ruleForm.sexTypes)&& this.flag !='users'){
            this.$message.error('性别不能为空');
            return
        }
      if('users'==this.flag && this.ruleForm.username.trim().length<1) {
        this.$message.error(`用户名不能为空`);
        return	
      }
      this.$http({
        url: `${this.$storage.get("sessionTable")}/update`,
        method: "post",
        data: this.ruleForm
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.$message({
            message: "修改信息成功",
            type: "success",
            duration: 1500,
            onClose: () => {
            }
          });
        } else {
          this.$message.error(data.msg);
        }
      });
    }
  }
};
</script>
<style lang="scss" scoped>
</style>
