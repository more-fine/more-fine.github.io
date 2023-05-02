---
title: jeecg-boot总结
date: 2020-09-18 23:28:32
tags:
---

# jeecgBoot总结

## 源码文件解读:

### 1.登录页面代码位置

```
src\components\layouts\UserLayout.vue
src/views/user/Login.vue
```

### 2.首页logo修改

```
 src/components/tools/Logo.vue
```

### 3.图片预览路径

```
public/index.html
<!-- 全局配置 -->
<script>
  window._CONFIG = {};
  window._CONFIG['imgDomainURL'] = 'http://localhost:8080/jeecg-boot/sys/common/view';
</script>
图文访问路径： http://127.0.0.1:8080/jeecg-boot/sys/common/view/user/h.jpg
```

### 4.首页报表

```
src/views/dashboard/*
src/views/dashboard/Analysis.vue
```

### 5.登录退出逻辑

```
 1.登录页面： src/views/user/Login.vue
 2.相关API定义位置： src/api/index.js（很多无用的删掉）
                    src/api/index.js
                    src/api/login.js
                    src/api/manage.js
 3.左侧菜单加载页面：src/components/menu
               src/utils/util.js
               src/permission.js
 4.隐藏路由配置
    用途： 如果那个组件不想在菜单上配置，但有需要路由跳转，则需要在这个地方配置路由。
    src/config/router.config.js
    对象： constantRouterMap
 5. 接口:   /sys/login        登录接口
            /sys/permission/queryByUser  获取用户信息接口（首页菜单）
```

### 6.首页风格设置

src/defaultSettings.js

### 7.首页样式自定义（ 统一修改样式 ）

src\assets\less\common.less



## 自定义组件

### 1.JDictSelectTag字典标签

提供了一个标签实现下拉和radio组件。JDictSelectTag 标签： 用于表单的标签使用，比如通过性别字典编码：sex，可以直接渲染出下拉组件。

```js
//第一步：
import JAreaLinkage from '@/components/jeecg/JAreaLinkage'
//导出：
components: {JAreaLinkage},

//运用标签
    //查询：
    //type="radio"单选框，不写就是下拉选择
    //v-model="queryParam.onShelves"查询字典表
		<j-dict-select-tag
                type="radio"
                v-model="queryParam.onShelves"
                placeholder="请选择状态"
                dictCode="goods_on_shelves"
              />
	//添加
    //:triggerChange="true"定义值可以被更改
    //v-decorator="['status',  {}]"绑定值===v-model
        <j-dict-select-tag
            v-decorator="['status',  {}]"
            :triggerChange="true"
            placeholder="当前状态"
            dictCode="website_news_status"
          />
```

### 2，时间引用

```js
//导入
import pick from 'lodash.pick'
import moment from 'moment'
//使用,createTime往后端传的字段
<a-date-picker showTime format="YYYY-MM-DD HH:mm:ss" v-decorator="[ 'createTime', {}]" />
    
 //edit方法里面修改时间格式
    // 时间格式化,输出到页面
     this.form.setFieldsValue({ createTime: this.model.createTime ? moment(this.model.createTime) : null })
```

### 3，富文本编辑器

#### 第一种：quillEdit

```js
//引用：
import quillEdit from '@/components/quillEdit'
//导出：
components: { quillEdit},
//使用:
    <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="文章内容">
          <quillEdit ref="quillEditForm" v-decorator="['deltaContent', {}]" @change="getEdit"></quillEdit>
        </a-form-item>
//触发富文本事件的方法：
getEdit(value, valuestr) {
      console.log(valuestr, value)
      this.model.deltaContent = JSON.stringify(value)
      this.model.content = valuestr
 },
//edit方法中
if (record.id) {
        // this.imageUrl = record.shopBossImg //编辑时改变给当前imageUrl变量
        // this.imageUrl2 = record.headFiles
        let id = record.id
        let that = this
        this.axios({
          url: '/admin/websiteNews/queryById?id=' + id,
          method: 'get'
        })
          .then(function(res) {
            //  setTimeout(hide, 0);
            // console.log(that.model, 'this.model')
            // console.log(res.result.summary)
            that.summary = res.result.summary //新闻摘要
            that.keywords = res.result.keywords //新闻关键字
            that.description = res.result.description //新闻描述
            // console.log(res, 'res=====')
            let fleg = JSON.stringify(res.result) == '{}'
            if (!fleg && res.result.content != '') {
              // console.log(111)
              res.result.deltaContent = JSON.parse(res.result.deltaContent)
              //富文本赋值
              that.$refs.quillEditForm.edit(res.result.deltaContent)
            }
          })
          .catch(function(err) {
            console.log(err)
          })
      } else {
        let that = this
        that.summary = ''
        that.keywords = ''
        that.description = ''
        // that.$refs.quillEditForm.edit('record.deltaContent')
      }

//提交时赋值
formData.deltaContent = JSON.stringify(this.model.deltaContent) //内容
```

**编辑器引入的js二次封装，解决图片上传base64格式，太耗性能问题，改用图片路径的格式存储：**代码如下

```js
<template>
  <div>
    <div class="quill-editor"></div>
    <div class="quill-editor-imgUpload">
      <a-upload
        name="file"
        :data="upData"
        :multiple="false"
        action="/admin/files/add"
        @change="handleChange"
      >
        <a-button id="checkImg" display="none"></a-button>
      </a-upload>
    </div>
  </div>
</template>

<script>
import Quill from 'quill'
import 'quill/dist/quill.snow.css'
//富文本图片拼接路径域名
// var dataUrl = "http://admin.hongtianjiaju.com/";//线上
var dataUrl = "http://192.168.3.68:8085/"//线下
export default {
  name: 'editor',
  props: {
    value: Object
  },
  data() {
    return {
      upData: {
        //展示图片
        subjectType: '08',
        fileType: '1',
        fileSubpath: 'quill' //图片路径
      },
      quill:null,
      options: {
        theme: 'snow',
        modules: {
            toolbar: [
              ['bold', 'italic', 'underline', 'strike'],
              ['blockquote', 'code-block'],
              [{ 'header': 1 }, { 'header': 2 }],
              [{ 'list': 'ordered' }, { 'list': 'bullet' }],
              [{ 'script': 'sub' }, { 'script': 'super' }],
              [{ 'indent': '-1' }, { 'indent': '+1' }],
              [{ 'direction': 'rtl' }],
              [{ 'size': ['small', false, 'large', 'huge'] }],
              [{ 'header': [1, 2, 3, 4, 5, 6, false] }],
              [{ 'color': [] }, { 'background': [] }],
              [{ 'font': [] }],
              [{ 'align': [] }],
              ['clean'],
              ['link', 'image', 'video']
            ]
          },
          placeholder: '请输入商品详情...'
      }
    }
  },


  methods:{
    gobacke(){
      let that=this
      if(that.formName!=''){
          that.$router.push({name:that.formName})
      }else{
          window.history.go(-1)
      }
    },

    edit(item){
       this.quill.setContents(item)
    },

    handleChange(info) {
      if (info.file.status !== 'uploading') {
        console.log(info.file, info.fileList);
      }
      if (info.file.status === 'done') {
        this.$message.success(`${info.file.name} 上传成功`);
        // 获取富文本组件实例
        let quill = this.quill
        // 如果上传成功
        // 获取光标所在位置
        let length = quill.getSelection().index;
        // 插入图片，res为服务器返回的图片链接地址
        if (!!info.file.response) {
          //如果存在这个参数显示
          let response = info.file.response
          //取出图片路径赋值显示
          quill.insertEmbed(length, 'image',dataUrl+response.result.fileUrl)
        }
        // 调整光标到最后
        quill.setSelection(length + 1)
      } else if (info.file.status === 'error') {
        this.$message.error(`${info.file.name} 上传失败`);
      }
    },

  },

  mounted() {
    let quillEditor = this.$el.querySelector('.quill-editor')
    this.quill = new Quill(quillEditor, this.options);
    this.quill.setContents(this.value)

    this.quill.on('text-change', () => {
      this.$emit('change', this.quill.getContents(),this.quill.container.firstChild.innerHTML)
    });

    var toolbar = this.quill.getModule('toolbar');
    toolbar.addHandler('image', ()=>{
      document.getElementById("checkImg").click();
    });
  }
}
</script>

<style scoped="scoped">
  .quill-editor{
    height: 500px;
  }

  .quill-editor-imgUpload {
    display: none;
  }


</style>

```

#### 第二种：JEditor



```js
//引入
import JEditor from '@/components/jeecg/JEditor'
//导出：
components: {
    editor: JEditor
  },
      
      
//添加方法
add() {
      this.edit({})
    },
//编辑方法
edit(record) {
      //////////////////////////////////////////
      this.findNewsCategoryList()
      this.form.resetFields()
      this.model = Object.assign({}, record)
      this.visible = true
      this.$nextTick(() => {
        this.form.setFieldsValue(
          pick(
            this.model,
            'title',
            'categoryId',
            'summary',
            'thumbnail',
            'browseCount',
            'keywords',
            'description',
            'status',
            'content'
          )
        )
        //时间格式化
      })
      if (record == null) {
        // 新增新闻
        this.model.thumbnail = this.picUrl
      } else {
        // 修改新闻
        this.picUrl = this.model.thumbnail
      }

      if (record.id) {
        let id = record.id
        let that = this
        this.axios({
          url: '/admin/news/queryById?id=' + id,
          method: 'get'
        })
          .then(function(res) {
            that.summary = res.result.summary //新闻摘要
            that.description = res.result.description //seo描述
            that.keywords = res.result.keywords //seo
            that.model.content = res.result.content //内容
            // console.log(that.model)
          })
          .catch(function(err) {
            console.log(err)
          })
      } else {
        let that= this;
        that.summary = '' //新闻摘要
        that.description = '' //seo描述
        that.keywords = '' //seo
        that.model.content = record.content //seo
      }
    },

```

**JEditor富文本js文件：解决base64存储太大问题二次封装：代码如下**

### 4，图片上传引用

```js
<!-- 图片上传 -->
        <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="图片">
          <a-upload
            name="file"
            list-type="picture-card"
            class="avatar-uploader"
            :show-upload-list="false"
            action="/admin/files/add"
            :before-upload="beforeUpload"
            @change="handleChange"
          >
            <div class="imageUrlshow">
              <img v-if="imageUrl" :src="imageUrl" alt="avatar" width="200px" />
              <div v-else>
                <a-icon :type="loading ? 'loading' : 'plus'" />
                <div class="ant-upload-text">上传</div>
              </div>
            </div>
          </a-upload>
</a-form-item>


data(){
    imageUrl: '', //存放img显示的变量
    loading: false, //正在加载
},
handleChange(info) {
      //change事件,图片上传中,上传成功,失败都会调用
      if (info.file.status === 'uploading') {
        //正在上传时
        this.loading = true //显示加载状态
        return
      }
      if (!!info.file.response) {
        //如果存在这个参数显示
        console.log(info)
        let response = info.file.response
        this.imageUrl = response.result.fileUrl //取出图片路径赋值显示
        this.model.userHead = response.result.fileUrl
        this.loading = false
      }
},
beforeUpload(file) {
      //图片上传前的回调函数
      const isJpgOrPng = file.type === 'image/jpeg' || file.type === 'image/png' || file.type === 'image/gif'
      if (!isJpgOrPng) {
        this.$message.error('头像图片只能上传jpg或者是png,gif的图片!')
        return false
      }
      const isLt2M = file.size / 1024 / 1024 < 2
      if (!isLt2M) {
        this.$message.error('头像图片不能超过2MB!')
        return false
	  }
},
edit(record) {
   // console.log(record)
   this.imageUrl = record.userHead //修改时赋值
    
    that.model = Object.assign({}, record)
      that.$nextTick(() => {
        that.form.setFieldsValue(
          pick(
            this.model,
            'userHead'
          )//userHead向后台传的图片变量
        )
      })
}

```

### 5，表格常用属性

```js
 		{
            title: '问题',
            align: "center",
            dataIndex: 'title',
            ellipsis: true,//溢出隐藏显示...
            width:'20%',
          },

```

### 6,文本域

```js
<a-textarea style="height:150px;" placeholder="请输入问题回答" v-decorator="['content', {} ]" />

```

### 7.单选框特殊性

```js
//Form 表单开发特殊性
//v-decorator 属性
//针对特殊控件： select、radio、checkbox

<a-radio-group buttonStyle="solid" v-decorator="[ 'status', {'initialValue':0}]">
    <a-radio-button :value="0">正常</a-radio-button>
    <a-radio-button :value="-1">停止</a-radio-button>
</a-radio-group>
//注意： 此处的默认值只能通过{'initialValue':0} 这样的设置，不能通过属性。
//表单编辑赋值操作：

this.$nextTick(() => {
    this.form.setFieldsValue(pick(this.model,'description','status'));
});

```

### 8，省市区三级联动

```js
//引入
import JAreaLinkage from '@comp/jeecg/JAreaLinkage'
//导出
components: {
    JAreaLinkage
},
<!-- 位置 -->
<a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="位置">
    <!-- 省市县级联 -->
    <j-area-linkage v-decorator="['location', {}]" type="select" />
</a-form-item>




```

### 9，时间选择器

```js
<a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="创建时间">
     <a-date-picker showTime format="YYYY-MM-DD HH:mm:ss" v-decorator="[ 'createTime', {}]" />
</a-form-item>
//添加
add() {
   this.edit({})
},
edit(record) {//编辑时调用的函数
  this.imageUrl = record.thumbnail //编辑时改变给当前imageUrl变量
  this.form.resetFields()
  this.model = Object.assign({}, record)
  // console.log(this.model, 'this.model')

      this.visible = true
      this.$nextTick(() => {
        this.form.setFieldsValue(
          pick(
            this.model,
            'title',
            'categoryType',
            'summary',
            'thumbnail'
          )
        )
        ////////////////////////////////////////////////////////////
        // 时间格式化,输出到页面
        this.form.setFieldsValue({ createTime: this.model.createTime ? moment(this.model.createTime) : null })
      })
    },
//表单验证
handleOk() {
      const that = this
      // 触发表单验证
      this.form.validateFields((err, values) => {
        if (!err) {
          that.confirmLoading = true
          // console.log(this.model)
          let httpurl = ''
          let method = ''
          if (!this.model.id) {
            httpurl += this.url.add
            method = 'post'
          } else {
            httpurl += this.url.edit
            method = 'put'
          }

          let formData = Object.assign(this.model, values)

		  // 时间格式化
          formData.createTime = formData.createTime ? formData.createTime.format('YYYY-MM-DD HH:mm:ss') : null
            
         httpAction(httpurl, formData, method)
            .then(res => {
              if (res.success) {
                that.$message.success(res.message)
                that.$emit('ok')
              } else {
                that.$message.warning(res.message)
              }
            })
            .finally(() => {
              that.confirmLoading = false
              that.close()
            })
        }
      })
    },

```

### 









# ant-design-vue总结

## input常用

### 1，文本域

```js
<a-textarea style="height:150px;" placeholder="请输入问题回答" v-decorator="['content', {} ]" />

```

### 2，input

```js
<a-input placeholder="请输入人员数量" v-decorator="['personNumber', {}]" />

```

### 3，单选框//配合jeecg-boot

```js
//编辑时赋值，修改
<a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="状态">
    <a-radio-group v-decorator="['status', {}]" > 
       <a-radio  :value="0">正常</a-radio>
       <a-radio :value="1">关闭</a-radio>
     </a-radio-group>
</a-form-item>

```



### 3，插槽使用，插入自定义显示内容

```js
//columns变量，绑定表格的标题及属性
<a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        @change="handleTableChange"
      >
       //插槽显示img，通过slot绑定插槽，slot-scope携带插槽内引用的数据
        <img
          style="width:34px;heigth:42px"
          slot="pic"
          slot-scope="text, record"
          :src="record.shopBossImg"
        />
        //插槽显示操作按钮方法
        <div style="min-width: 100px" slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>&nbsp;
          <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
            <a>删除</a>
          </a-popconfirm>

<!--   <a-divider type="vertical" />//一条竖线分割按钮的
         <a-dropdown>
           <a class="ant-dropdown-link">
             更多
             <a-icon type="down" />
           </a>
           <a-menu slot="overlay">
             <a-menu-item>-->
               <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                 <a>删除</a>
                </a-popconfirm>
             </a-menu-item>
           </a-menu>
         </a-dropdown>-->
        </div>
</a-table>



data(){
    return{
        columns: [
        {//表格第一列前的序号
          title: '#',
          dataIndex: '',
          key: 'rowIndex',
          width: 60,
          align: 'center',
          customRender: function(t, r, index) {
            return parseInt(index) + 1
          }
        },
        {
          title: '头像',
          align: 'center',
          dataIndex: 'shopBossImg',
          key: 'pic',
          scopedSlots: { customRender: 'pic' }
        },
        {//action插槽名称
          title: '操作',
          width: 150,
          dataIndex: 'action',
          align: 'center',
          scopedSlots: { customRender: 'action' }
        },
         {
            title: '支付渠道',
            align: "center",
            dataIndex: 'payChannel',
            customRender: function(text) {
              if (text == 1) {
                return '微信支付'
              } else if (text == 2) {
                return '支付宝支付'
              } else if (text == 3) {
                return '线下支付'
              } 
            }
          },
      ],
    }
}

```

### 4，显示头像的展示

参数为空自动显示默认头像的img

```js
<a-avatar src="图片路径" />

```

### 5，金额格式化

```js
<div class="pressNum" slot="factoryPrice" slot-scope="text, record">
          <!-- //商品出厂价 -->
          <a-statistic valueStyle="font-size:16px;" :precision="2" :value="record.factoryPrice" />
          <!-- <div>元</div> -->
</div>

```

### 6，模态框调用，父子组件传值，以及逻辑业务处理demo

**预览时：**引入子组件，进行父===》子组件的传值，查找数据 ，进行渲染，ref方法传值

**点击发货请求时：**调用模态框进行逻辑业务处理 

**父组件：**

```vue
<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">
          <a-col :md="6" :sm="8">
            <a-form-item label="订单编号">
              <a-input placeholder="订单编号" v-model="queryParam.orderNo"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="收货人电话">
              <a-input placeholder="请输入手机号" v-model="queryParam.consigneeTelephone"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="订单状态">
              <j-dict-select-tag
                v-model="queryParam.orderGoodsStatus"
                placeholder="当前状态"
                dictCode="order_status "
              />
            </a-form-item>
          </a-col>

          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button
                type="primary"
                @click="searchReset"
                icon="reload"
                style="margin-left: 8px"
              >重置</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
	<!-- 查询区域end -->
      
    <!-- table区域-begin -->
    <div>
      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        @change="handleTableChange"
      >
        <img
          style="width:50px;heigth:50px"
          slot="files"
          slot-scope="text, record"
          :src="record.files[0].fileUrl"
        />
      
        <!-- 插槽按钮 -->
        <span slot="action" slot-scope="text, record">
          <!-- 查看详情按钮 -->
          <a @click="orderView(record)">详情</a>
          <!-- 发货按钮 -->
          <span>
            <a-divider type="vertical" />
            <a @click="deliverShow(record)">发货</a>
          </span>
        </span>
      </a-table>
    </div>
    <!-- table区域-end -->
      
      
    <!-- 模态弹框 -->
    <a-modal
      :title="itemObj.orderNo"
      width="416px"
      :visible="deliverWin"
      :closable="false"
      @ok="handleOk"
      :confirmLoading="confirmLoading"
      @cancel="handleCancel"
    >
      <a-form :form="form" :label-col="{ span: 5 }" :wrapper-col="{ span: 19 }">
        <a-form-item label="物流公司">
          <a-input
            placeholder="请输入物流公司名称"
            v-decorator="['logisticsCompany', { rules: [{ required: true, message: '请输入物流公司名称!' }] }]"
          />
        </a-form-item>

        <a-form-item label="物流单号">
          <a-input
            placeholder="请输入物流单号"
            v-decorator="['logisticsNo', { rules: [{ required: true, message: '请输入物流公司单号!' }] }]"
          />
        </a-form-item>

        <a-form-item label="物流备注">
          <a-textarea placeholder="请输入物流备注" v-decorator="['logisticsRemark', {  }]" />
        </a-form-item>
      </a-form>
    </a-modal>
    <!-- 模态弹框end -->
	 <!--子组件的调用-->
    <details-model ref="detailsView"></details-model>
  </a-card>
</template>

<script>
import Vue from 'vue'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'
import { USER_INFO } from '@/store/mutation-types'
//子组件的引入
import detailsModel from './modules/detailsModel'
export default {
  name: '',
  mixins: [JeecgListMixin],
  components: {
	//子组件的挂载
    detailsModel
  },

  data() {
    return {
      description: '',
      disableMixinCreated: true,
      userInfo: {},
      confirmLoading: false,
      form: this.$form.createForm(this, {}),
      itemObj: {},
      deliverWin: false,
      // 表单数据，部分省略
      columns: [
        {
          title: '订单编号',
          align: 'center',
          dataIndex: 'orderNo'
        },

        {
          title: '下单人',
          align: 'center',
          dataIndex: 'sysAccountName'
        }
      ],
      url: {
        list: '/admin/caseInfo/list',
        delete: '/admin/caseInfo/delete',
        deleteBatch: '/admin/caseInfo/deleteBatch',
        exportXlsUrl: 'admin/caseInfo/exportXls',
        importExcelUrl: 'admin/caseInfo/importExcel'
      }
    }
  },
  computed: {
    importExcelUrl: function() {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`
    }
  },
  methods: {
    deliverShow(item) {
      this.itemObj = item
      this.form.resetFields()
      this.deliverWin = true
    },

    orderView(item) {
      this.$refs.detailsView.view(item)
    },

    handleOk(e) {//确认按钮的事件，给后台传递数据
      var newDate = new Date()

      var year = newDate.getFullYear()
      var month = newDate.getMonth() < 10 ? '0' + (newDate.getMonth() + 1) : newDate.getMonth() + 1
      var day = newDate.getDate() < 10 ? '0' + newDate.getDate() : newDate.getDate()

      var h = newDate.getHours() < 10 ? '0' + newDate.getHours() : newDate.getHours()
      var m = newDate.getMinutes() < 10 ? '0' + newDate.getMinutes() : newDate.getMinutes()

      var s = newDate.getSeconds() < 10 ? '0' + newDate.getSeconds() : newDate.getSeconds()

      e.preventDefault()
      this.form.validateFields((err, values) => {
        if (!err) {
          values.id = this.itemObj.id
          values.orderGoodsStatus = 2
          values.deliveryTime = year + '-' + month + '-' + day + ' ' + h + ':' + m + ':' + s
          const hide = this.$message.loading('正在提交物流信息..', 0)
          this.confirmLoading = true
          var that = this
          this.axios({
            url: '/admin/bsOrderGoods/send',
            method: 'put',
            data: values
          })
            .then(function(res) {
              setTimeout(hide, 0)
              if (res.success) {
                that.$message.success('物流信息提交成功')
                that.deliverWin = false
                that.searchQuery()

                that.confirmLoading = false
              }
            })
            .catch(function(err) {
              setTimeout(hide, 0)
              that.confirmLoading = false
              console.log(err)
            })
        }
      })
    },

    handleCancel() {
      this.deliverWin = false
    },

    getOrder() {
      this.url.list = '/admin/bsOrderGoods/list' 
      this.searchQuery()
    }
  },

  mounted() {
    this.userInfo = Vue.ls.get(USER_INFO)
  console.log(this.userInfo,'this.userInfo')
    this.getOrder()
  },

  beforeRouteEnter(to, from, next) {
    console.log(to, from, next)
    // 注意，在路由进入之前，组件实例还未渲染，所以无法获取this实例，只能通过vm来访问组件实例
    next(vm => {
      if (!!from) {
        vm.userInfo = Vue.ls.get(USER_INFO)
        vm.getOrder()
      }
    })
  }
}
</script>
<style scoped>
/* 引入css文件*/
@import '~@assets/less/common.less';
</style>


```

**子组件：**

```vue

```

### 7，input必选，以及提示

```js
<a-input placeholder="请输入物流单号" v-decorator="['logisticsNo', { rules: [{ required: true, message: '请输入物流公司单号!' }] }]"
/>
    
//第二种
<a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="案例标题">
   <a-input placeholder="请输入案例标题" v-decorator="['title', validatorRules.title]" />
</a-form-item>
validatorRules: {
   title: {
       rules: [
         {
           required: true,
           message: '请输入案例标题!'
          }
       ]
   },
}

```

### 8，图片预览

```js
//预览的组件，ant框架
<a-modal :visible="previewVisible" :footer="null" @cancel="yulanVisible">
    <img alt="example" style="width: 100%" :src="previewImage" />
</a-modal>
yulanVisible() {
  this.previewVisible = false;
},
handlePreview(){//绑定a-upload图片上传的事件@preview="handlePreview"
  //预览图片时的回调函数
  // console.log(file)
  this.previewImage = file.url
  this.previewVisible = true
}

```

### 9，删除------确定按钮

```js
//更多滑过显示删除，点击删除有弹出二次确认
<a-divider type="vertical" />
          <a-dropdown>
            <!-- <a class="ant-dropdown-link">更多 <a-icon type="down"/></a> -->
            <!-- <a-menu slot="overlay"> -->
              <!-- <a-menu-item> -->
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              <!-- </a-menu-item> -->
            <!-- </a-menu> -->
          </a-dropdown>

```



### 10,vue引入组件

```js
//点击事件弹出模态框record传递参数
<a @click="orderView(record)">详情</a>
//引入
import detailsModel from './modules/detailsModel'
//导出：
components: {
    detailsModel,
},

//引用
<details-model ref="detailsView"></details-model>
    
methods:{
    orderView(item) {
      this.$refs.detailsView.view(item)
    },
}

```

