<template>
  <el-dialog
    title="效果预览"
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
</template>
<script>
export default {
  name: "PreviewCode",
  props: {
    dialogVisible: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      tableData: [
        {
          name: "大数据应用平台",
          account: "346802579032",
          type: "数据分析类",
        },
        {
          name: "数据分析平台",
          account: "673948752812",
          type: "应用维护类",
        },
        {
          name: "报表分析平台",
          account: "587089276489",
          type: "报表分析类",
        },
        {
          name: "智能生成系统",
          account: "679880276482",
          type: "智慧应用类",
        },
        {
          name: "大数据应用平台",
          account: "346802579032",
          type: "数据分析类",
        },
        {
          name: "数据分析平台",
          account: "673948752812",
          type: "应用维护类",
        },
        {
          name: "报表分析平台",
          account: "587089276489",
          type: "报表分析类",
        },
        {
          name: "智能生成系统",
          account: "679880276482",
          type: "智慧应用类",
        },
        {
          name: "大数据应用平台",
          account: "346802579032",
          type: "数据分析类",
        },
        {
          name: "数据分析平台",
          account: "673948752812",
          type: "应用维护类",
        },
      ],
    };
  },
  methods: {
    handleClose(done) {
      done();
      this.$emit("close");
    },
  },
};
</script>
<style scoped lang="less">
/deep/ .page-generator_code-form {
  &-select {
    .el-input__inner {
      width: 240px;
      height: 32px;
    }
  }
  &-input {
    .el-input__inner {
      width: 240px;
      height: 32px;
    }
  }
  &-button {
    .el-button {
      width: 65px;
      height: 32px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #fff;
      background: #ff7635 !important;
      border: none;
      padding: 0;
      transform: translateY(2px);
    }
  }
}

.page-generator_code-table {
  .el-icon-edit-outline {
    padding-right: 8px;
    color: #ff7635;
    border-right: 1px solid#E9E9E9;
  }
  .el-icon-delete {
    padding-left: 8px;
    color: red;
  }
}

.page-generator_code-pagination {
  text-align: right;
  margin-top: 10px;
  /deep/ .el-pagination {
    .btn-prev,
    .btn-next,
    .number,
    .el-icon-more {
      background: #fff;
      border: 1px solid #dcdfe6;
      border-radius: 4px;
      &.active {
        background: #ff7635 !important;
        border-color: #ff7635 !important;
        color: #fff !important;
      }
    }
    .el-icon-d-arrow-left,
    .el-icon-d-arrow-right {
      color: #ff7635 !important;
    }
    .btn-prev,
    .btn-next {
      &:hover {
        color: #ff7635;
      }
      &[disabled="disabled"] {
        color: #c0c4cc !important;
      }
    }
  }
  /deep/ .el-pagination.is-background .el-pager li:not(.disabled):hover {
    color: #ff7635;
  }
}
</style>