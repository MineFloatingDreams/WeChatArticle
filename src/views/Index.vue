<template>
  <div id="index">
    <div>
      <h1>微信公众号文章生成器</h1>
    </div>
    <el-divider></el-divider>
    <el-tabs>
      <el-tab-pane label="样式设置">
        <div>
          <el-row :gutter="40">
            <el-col :span="12">
              <h2>
                一级标题样式
                <el-button @click="saveTitle1" style="float: right" class="el-icon-s-order" type="primary"
                           circle></el-button>
              </h2>
              <p>请将需要替换的文字改为<span style="color: red">一级标题</span></p>
              <el-input
                type="textarea"
                :autosize="{ minRows: 4, maxRows: 8}"
                placeholder="请输入内容"
                v-model="title1">
              </el-input>
            </el-col>
            <el-col :span="12">
              <h2>样式预览</h2>
              <div v-html="title1"></div>
            </el-col>
          </el-row>

          <el-row :gutter="40">
            <el-col :span="12">
              <h2>
                二级标题样式
                <el-button @click="saveTitle2" style="float: right" class="el-icon-s-order" type="primary"
                           circle></el-button>
              </h2>

              <p>请将需要替换的文字改为<span style="color: red">二级标题</span></p>
              <el-input
                type="textarea"
                :autosize="{ minRows: 4, maxRows: 8}"
                placeholder="请输入内容"
                v-model="title2">
              </el-input>
            </el-col>
            <el-col :span="12">
              <h2>样式预览</h2>
              <div v-html="title2"></div>
            </el-col>
          </el-row>

          <el-row :gutter="40">
            <el-col :span="12">
              <h2>
                三级标题样式
                <el-button @click="saveTitle3" style="float: right" class="el-icon-s-order" type="primary"
                           circle></el-button>
              </h2>
              <p>请将需要替换的文字改为<span style="color: red">三级标题</span></p>
              <el-input
                type="textarea"
                :autosize="{ minRows: 4, maxRows: 8}"
                placeholder="请输入内容"
                v-model="title3">
              </el-input>
            </el-col>
            <el-col :span="12">
              <h2>样式预览</h2>
              <div v-html="title3"></div>
            </el-col>
          </el-row>

          <el-row :gutter="40">
            <el-col :span="12">
              <h2>
                正文样式
                <el-button @click="saveContextStyle" style="float: right" class="el-icon-s-order" type="primary"
                           circle></el-button>
              </h2>

              <p>请将需要替换的文字改为<span style="color: red">正文内容</span></p>
              <el-input
                type="textarea"
                :autosize="{ minRows: 8, maxRows: 12}"
                placeholder="请输入内容"
                v-model="context_style">
              </el-input>
            </el-col>
            <el-col :span="12">
              <h2>样式预览</h2>
              <div v-html="context_style"></div>
            </el-col>
          </el-row>
        </div>
        <div style="padding-top: 30px">
          <el-button type="primary" @click="saveAll">全部保存</el-button>
        </div>
      </el-tab-pane>
      <el-tab-pane label="文章内容">
        <div style="margin-top: 20px">
          <mavon-editor v-model="context" :toolbars="toolbars"/>

          <div style="margin-top: 20px">
            <el-button type="primary" @click="saveContext">保存</el-button>
            <el-button @click="reset">重置</el-button>
          </div>
        </div>


      </el-tab-pane>
      <el-tab-pane label="文章预览">
        <div v-html="htmlText">

        </div>
      </el-tab-pane>
      <el-tab-pane label="代码复制">
        <div style="padding-top: 30px">
<!--          复制到剪切板-->
          <el-button type="primary" @click="copy">复制</el-button>
          <el-switch
            v-model="divider"
            active-text="显示下划线"
            inactive-text="关闭下划线">
          </el-switch>
        </div>
        <div v-for="text in html">
          <el-divider v-if="divider" content-position="left">{{text.type}}</el-divider>
          <div style="padding-top: 30px" v-else></div>
          {{ text.text }}
        </div>
      </el-tab-pane>
    </el-tabs>


  </div>
</template>

<script>
export default {
  name: "Index",
  data() {
    return {
      title1: '',
      title2: '',
      title3: '',
      context_style: '',
      context: ' ',//输入的数据
      toolbars: {
        bold: true, // 粗体
        italic: true, // 斜体
        header: true, // 标题
        underline: true, // 下划线
        mark: false, // 标记
        superscript: false, // 上角标
        quote: false, // 引用
        ol: false, // 有序列表
        link: false, // 链接
        imagelink: true, // 图片链接
        help: true, // 帮助
        code: false, // code
        subfield: true, // 是否需要分栏
        fullscreen: false, // 全屏编辑
        readmodel: false, // 沉浸式阅读
        undo: true, // 上一步
        trash: true, // 清空
        save: false, // 保存（触发events中的save事件）
        navigation: false // 导航目录
      },
      html: [],
      htmlText: '',
      divider:true,
    }
  },
  created() {
    this.title1 = localStorage.getItem('title1')
    this.title2 = localStorage.getItem('title2')
    this.title3 = localStorage.getItem('title3')
    this.context_style = localStorage.getItem('context_style')
    this.context = localStorage.getItem('context')
  },
  methods: {
    saveTitle1() {
      localStorage.setItem('title1', this.title1)
    },
    saveTitle2() {
      localStorage.setItem('title2', this.title2)
    },
    saveTitle3() {
      localStorage.setItem('title3', this.title3)
    },
    saveContextStyle() {
      localStorage.setItem('context_style', this.context_style)
    },
    reset() {
      this.context = ''
    },
    saveContext() {
      localStorage.setItem('context', this.context)
      const htmlContent = this.converter.makeHtml(this.context);
      let dom = document.createElement('div')
      dom.innerHTML = htmlContent
      let tempHtml = [];
      for (let i = 0; i < dom.children.length; i++) {
        tempHtml.push({
          type: dom.children[i].nodeName,
          text: this.changeHtml(dom.children[i])
        })
      }
      this.html = tempHtml;
      let tempText = ''
      tempHtml.forEach(item => {
        tempText += item.text
      });
      this.htmlText = tempText
    },
    saveAll() {
      this.saveTitle1();
      this.saveTitle2();
      this.saveTitle3();
      this.saveContextStyle();
    },
    changeHtml(html) {
      let temp = '';
      let nodeName = html.nodeName;
      let innerHTML = html.innerHTML;
      if (nodeName === 'H1') {
        temp = this.title1.replace('一级标题', innerHTML)
      } else if (nodeName === 'H2') {
        temp = this.title2.replace('二级标题', innerHTML)
      } else if (nodeName === 'H3') {
        temp = this.title3.replace('三级标题', innerHTML)
      } else if (nodeName === 'P') {
        temp = this.context_style.replace('正文内容', innerHTML)
      }
      return temp;
    },
    copy() {
      const url = this.htmlText;
      const oInput = document.createElement('input');
      oInput.value = url;
      document.body.appendChild(oInput);
      oInput.select(); // 选择对象;
      console.log(oInput.value)
      document.execCommand('Copy'); // 执行浏览器复制命令
      this.$message({
        message: '复制成功',
        type: 'success'
      });
      oInput.remove()
    },
  },
}
</script>

<style scoped>
#index {
  width: 80%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);
  padding: 30px;
  margin: 30px auto 50px;
}
</style>
