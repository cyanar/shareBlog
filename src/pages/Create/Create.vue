<template>
  <div id="edit">
    <h1>创建文章</h1>
    <h3>文章标题</h3>
    <el-input v-model="title" class="title"></el-input>
    <p class="msg">限30个字</p>
    <h3>内容简介</h3>
    <el-input type="textarea" class="description" v-model="description" :autosize="{ minRows: 2, maxRows: 6}"></el-input>
    <p class="msg">限300个字</p>
    <h3>文章内容</h3>
    <el-input type="textarea" class="content" v-model="content" :autosize="{ minRows: 4, maxRows: 30}"></el-input>
    <p class="msg">限3000个字</p>
    <p class="ifShow">
      <label>是否展示到首页</label>
      <el-switch v-model="ifShow" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
    </p>
    <el-button @click="onCreate" class="confirm">确定</el-button>
  </div>
</template>
<script>
import fetch from '@/components/fetch.js';
export default {
  data() {
    return {
      title: '',
      description: '',
      content: '',
      ifShow: true,
    };
  },
  methods: {
    notice(content, type) {
      this.$message({
        showClose: true,
        message: content,
        type: type,
      });
    },
    onCreate() {
      if (this.title && this.description && this.content) {
        var publishTime = new Date();
        var saveTime = publishTime.getTime();
        fetch
          .createArtical(
            this.$store.state.user,
            this.title,
            this.description,
            this.content,
            saveTime,
            this.$store.state.iconUrl,
            this.$store.state.user,
            this.$store.state.dearName,
            this.ifShow
          )
          .then(() => {
            this.notice('保存成功', 'success');
            this.title = '';
            this.description = '';
            this.content = '';
            this.$router.push('/main');
          });
        if (this.ifShow) {
          fetch.createArtical(
            'allArtical',
            this.title,
            this.description,
            this.content,
            saveTime,
            this.$store.state.iconUrl,
            this.$store.state.user,
            this.$store.state.dearName,
            this.ifShow
          );
        }
      } else {
        if (!this.title) {
          this.notice('标题不能为空', 'warning');
        }
        if (!this.description) {
          this.notice('简介不能为空', 'warning');
        }
        if (!this.content) {
          this.notice('文章内容不能为空', 'warning');
        }
      }
    },
  },
};
</script>   
<style lang="less">
@import '../../assets/base.less';

#edit {
  display: flex;
  padding: 0 5%;
  flex-direction: column;
  height: 100%;
  h1 {
    text-align: center;
  }
  h3 {
    font-size: 24px;
  }
  .title {
    font-size: 30px;
  }
  .description {
    font-size: 22px;
  }
  .content {
    font-size: 18px;
  }
  .msg {
    text-align: right;
    font-size: 16px;
    color: #999;
  }
  .confirm {
    color: @bgColor;
  }
  .confirm:hover {
    color: white;
    background-color: @bgColor;
  }
  .ifShow {
    padding: 10px;
  }
}
</style>
