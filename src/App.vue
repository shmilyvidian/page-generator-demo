<template>
  <div id="app">
    <div class="page-generator_content">
      <el-row>
        <el-col :span="12">
          <div class="page-generator_left">
            <div class="page-generator_upload">
              <el-button v-if="status === 0" size="big" type="primary" @click="status = 1">点击上传</el-button>
              <div :class="status === 1 ? 'page-generator_upload-picture' : ''" v-else>
                <img :src="require('./assets/test.png')" width="540" height="360"/>
                <span class="page-generator_upload-loadingText">解析中...<i class="el-icon-loading"></i></span>
                <i class="el-icon-close" @click="status = 0" v-if="status !== 0"></i>
              </div>
            </div>
            <img :src="require('./assets/programmer.gif')" class="page-generator_programmer" width="300" height="156" alt="">
            <el-dialog
              title="设计稿预览"
              :visible.sync="picVisible"
              width="50%"
              :before-close="handlePicClose"
            >
              <img :src="require('./assets/test.png')" alt="" srcset="" width="100%" height="100%">
            </el-dialog>
          </div>
        </el-col>
        <el-col :span="12">
          <div class="page-generator_right">
            <div class="page-generator_command">
              <vue-command :status="status" @changStatus="changStatus" />
            </div>
            <div class="page-generator_code">
              <editor
                v-model="content"
                ref="myEditor"
                @init="editorInit"
                lang="html"
                theme="chrome"
                width="100%"
              />
              <div class="page-generator_code-preview" v-if="status === 3">
                <el-button @click="dialogVisible = true">预览</el-button>
              </div>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
    <preview-code :dialogVisible="dialogVisible" @close="handleClose"/>
  </div>
</template>

<script>
import VueCommand from "./components/VueCommand.vue";
import PreviewCode from "./components/PreviewCode.vue";

const content = `
        <template>
          <el-dialog
            title="预览"
            :visible.sync="dialogVisible"
            width="65%"
            :before-close="handleClose"
          >
            <div class="page-generator_code-form">
              <el-form :inline="true">
                <el-form-item class="page-generator_code-form-select">
                  <el-select v-model="value" placeholder="全部类型">
                    <el-option
                      v-for="item in options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    />
                  </el-select>
                </el-form-item>
                <el-form-item class="page-generator_code-form-input">
                  <el-input v-model="input" placeholder="请输入关键词搜索"></el-input>
                </el-form-item>
                <el-form-item class="page-generator_code-form-button">
                  <el-button>查询</el-button>
                </el-form-item>
              </el-form>
              <div class="page-generator_code-table">
                <el-table
                  :data="tableData"
                  style="width: 100%"
                  :header-cell-style="{ background: '#F5F7FA', color: '#606266' }"
                >
                  <el-table-column prop="name" label="应用名称"> </el-table-column>
                  <el-table-column prop="account" label="系统编号"> </el-table-column>
                  <el-table-column prop="type" label="应用类型"> </el-table-column>
                  <el-table-column label="操作">
                    <template>
                      <i class="el-icon-edit-outline"></i>
                      <i class="el-icon-delete"></i>
                    </template>
                  </el-table-column>
                </el-table>
              </div>
              <div class="page-generator_code-pagination">
                <el-pagination background layout="prev, pager, next" :total="100">
                </el-pagination>
              </div>
            </div>
          </el-dialog>
        </template>`;
export default {
  name: "App",
  components: {
    VueCommand,
    PreviewCode,
    editor: require("vue2-ace-editor"),
  },
  data() {
    return {
      dialogVisible: false,
      picVisible: false,
      status: 0,
      fileList: [],
      value: "",
      content: "",
    };
  },
  methods: {
    handleClose(){
      this.dialogVisible = false
    },
    handlePicClose(){
      this.picVisible = false
    },
    changStatus(type) {
      this.status = type;
      if (type === 3) {
        this.content = content;
      }
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      this.picVisible = true
      console.log('enter', file);
    },
    handleError() {
      this.fileList = [
        {
          name: "test.png",
          url: require("./assets/test.png"),
        },
      ];
      this.status = 2;
    },
    editorInit: function (editor) {
      editor.getSession().setUseWorker(false);
      require("brace/ext/language_tools"); //language extension prerequsite...
      require("brace/mode/html");
      require("brace/mode/javascript"); //language
      require("brace/mode/less");
      require("brace/theme/chrome");
      require("brace/snippets/javascript"); //snippet
    },
    
  },
};
</script>

<style lang='less'>
@import "./app.less";
@import "./styles/element-ui.less";

</style>
