

# fronted update log and created by 2019-01-23
   > 当前使用的组件以及库版本   [![vuejs](https://img.shields.io/badge/vue-2.15.17-brightgreen.svg?style=flat-square)](https://cn.vuejs.org/)      [![vuejs](https://img.shields.io/badge/iview-2.13.0-brightgreen.svg?style=flat-square)](https://cn.vuejs.org/)

- ## Change feature
 - 添加了一个Mixins全局混合对象,具体参见 [minxins.js](http://code.hoau.net/itiaoling-if/rol-face/blob/master/src/mixins/mixin.js)
  >- 混入全局的vuex
  >- 混合对象中添加了表格支持render省市区,render数据字典内容,获取指定时间范围的全局方法
 - 添加 `DictionarySelector.vue` 按需加载属性autoLoad,
 - 修改 `DictionarySelector.vue` 不需要声明dictionaryInstance实例, 见例子 [homePage/index.vue](http://code.hoau.net/itiaoling-if/rol-face/blob/master/src/components/homePage/index.vue)
 - 修改 `GeneralTable.vue` 父组件属性为tableColumns, 见例子 [homePage/index.vue](http://code.hoau.net/itiaoling-if/rol-face/blob/master/src/components/homePage/index.vue)
- ## Trash feature
 - 移除`DynamicSelector.vue`, `StaticSelector.vue` (下拉框直接使用[iView](http://v2.iviewui.com/components/select)`<Select>` 或 `<i-select>`)
- ## Bug fixed
 - 解决在未登录情况下,切换不了国际化的问题
 - 解决在未登录的情况下,左侧菜单一直在加载中的问题
- ## Show Case

  **下面是页面的简单例子 [homePage/index.vue](http://code.hoau.net/itiaoling-if/rol-face/blob/master/src/components/homePage/index.vue)**

  ```javascript
   <script type="text/babel">
     export default {
       name: 'Main',
       data () {
         return {
           queryModel: {
             name: '',
             age: '',
             sex: '',
             birthday: this.generalInitDateTime(-1, 'm'),
             district: [],
             createTime: []
           },
           columns: [
             {
               title: 'Name',   //需要国际化内容 this.$t('key')
               key: 'name'
             },
             {
               title: 'Age',
               key: 'age'
             },
             {
               title: 'Sex',
               key: 'sex'
             },
             {
               title: 'Birthday',
               key: 'birthday'
             },
             {
               title: 'District',
               key: 'district'
             },
             {
               title: 'CreateTime',
               key: 'createTime'
             }
           ]
         }
       },
       computed: { // 计算属性
         // ...mapGetters  已不需要声明
       },
       watch:{ // 监听属性

       },
       methods: {  // 方法属性
         handlerSelectChange () {

         },
         departureChange () {

         }
       },
       mounted () { // onReady,dom加载后执行
         this.queryModel.createTime = this.generalInitDateTime(-1, 'm')
       }
     }
   </script>
  ```
  ```html5
    <template>
     <!-- 定义统一风格 (必选) -->
     <div class="app-container">
       <!-- 查询条件(可选) -->
       <div class="filter-container">
         <i-form :model="queryModel" :label-width="95">
           <!--bootstrap 栅格布局-->
           <div class="row">
             <div class="col-sm-6 col-md-6 col-lg-3">
               <i-form-item label="Name" props="name">
                 <i-input v-model="queryModel.name"></i-input>
               </i-form-item>
             </div>

             <div class="col-sm-6 col-md-6 col-lg-3">
               <i-form-item label="Sex" props="sex">
                 <dictionary-selector
                   v-model="queryModel.sex"
                   dictionaryCode="DICTIONARY_SEX"
                   isShowAllOption
                   autoLoad>
                 </dictionary-selector>
               </i-form-item>
             </div>

             <div class="col-sm-6 col-md-6 col-lg-3">
               <i-form-item label="Age" props="age">
                 <i-input v-model="queryModel.age"></i-input>
               </i-form-item>
             </div>

             <div class="col-sm-6 col-md-6 col-lg-3">
               <i-form-item label="Birthday" props="birthday">
                 <DatePicker
                   type="daterange"
                   v-model="queryModel.birthday"
                   placement="bottom-end"
                   :clearable="false"
                   :editable="false">
                 </DatePicker>
               </i-form-item>
             </div>

             <div class="col-sm-12 col-md-12 col-lg-4">
               <Form-item label="District" prop="district">
                 <area-selector
                   @on-change="departureChange"
                   v-model="queryModel.district"
                   :level="2"
                   searchable>
                 </area-selector>
               </Form-item>
             </div>

             <div class="col-sm-6 col-md-6 col-lg-3">
               <i-form-item label="CreateTime" props="createTime">
                 <date-time-multiple
                   ref="createTime"
                   v-model="queryModel.createTime"
                   type="daterange"
                   :format="`yyyy-MM-dd HH:mm:ss`"
                   :editable="false"
                   :transfer="false"
                   defaultValue>
                 </date-time-multiple>
               </i-form-item>
             </div>

           </div>
         </i-form>
       </div>
       <!--主体内容div (必选)-->
       <div class="panel-container">
         <!--功能按钮 (可选)-->
         <div class="row filter-btn" align="right"></div>
         <!--通用表格 (可选)-->
         <general-table :tableColumns="columns" ref="generalGrid" @selectChange="handlerSelectChange"></general-table>
       </div>
     </div>
    </template>
  ```

  > **小规范**: <br>
   - 上述case中,因为已经全局引入所以不需要再使用`import { mapGetters } from 'vuex'`, `computed`属性中的`...mapGetters`也不需要定义<br>
   - 每个组件必须有name属性且首字母必须大写<br>
   - mixins.js中的全局属性或方法可以直接使用`this.`调用
