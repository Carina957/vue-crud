# Crud

![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/vBPlNzp2PA3XldG8/img/bf763874-cb4b-4d1e-808d-d4799c1b775f.png)[]

## [**What is CRUD?**](https://www.freecodecamp.org/news/crud-operations-explained/)

_CRUD refers to the four basic operations a software application should be able to perform – Create, Read, Update, and Delete._

> CRUD  指的是软件应用程序应该能够执行的四个基本操作——创建、读取、更新和删除。

## Directory

    ├── components # submodule foler
    │   └── Crud
    │       ├── crud.js # crud 核心逻辑
    │       ├── CRUD.operation.vue # 对多选表格的批量操作
    │       ├── Pagination.vue # 分页组件
    │       ├── RR.operation.vue # 表格筛选组件
    │       ├── UD.operation.vue # 表格单项删除组件
    │       └── UDOPT.operation.vue # 表格单项操作

## crud.js

**Crud**  的核心逻辑，共计九个函数，最重要的就是  **CRUD(options)**  函数，封装了配置项、方法、钩子函数。

- CRUD(options)  包含主要功能的公共方法

- callVmHook(crud, hook)  调用钩子函数

- mergeOptions(src, opts)  合并配置项，将传入的参数合并到当前实例中

- presenter(crud)  生成  crud

- header()  头部

- pagination()  分页

- form(defaultForm)  表单

- crud(options = {})  初始化  Crud

## Usage

    <script>
    import CRUD, { presenter, header, form, crud } from '@crud/crud'
    import rrOperation from '@crud/RR.operation'
    import pagination from '@crud/Pagination'
    import crudReq from '@/api/xxx/xxx.js'

    const defaultForm = {
      ...
    }

    export default {
      components: { rrOperation, pagination },
      cruds() {
        return CRUD({
          title: 'xxx',
          url: `/xxx/xxx`,
          query: { xxx: 'xxx', xxx: 'xxx', ... },
          crudMethod: { ...crudReq }
        })
      },
      mixins: [presenter(), header(), form(defaultForm), crud()],
    }
    </script>

## Configure

### CRUD Attributes

| **参数**                | **说明**                         | **类型** | **可选值**                                      | **默认值**                                                       |
| ----------------------- | -------------------------------- | -------- | ----------------------------------------------- | ---------------------------------------------------------------- |
| tag                     | 同一页面中多个  crud  实例的标识 | String   | \---                                            | default                                                          |
| idField                 | 接口响应对象  id  字段名         | String   | \---                                            | id                                                               |
| title                   | 当前  crud  示例的标题           | String   | \---                                            | \---                                                             |
| url                     | 请求数据的  url                  | String   | \---                                            | \---                                                             |
| reqType                 | 请求接口的方法                   | String   | get, post, delete, put, head, options           | get                                                              |
| data                    | 表格数据                         | Array    | \---                                            | \[\]                                                             |
| infoDia                 | 表格项是否可编辑                 | Boolean  | \---                                            | false                                                            |
| selections              | 表格选中项                       | Array    | \---                                            | \[\]                                                             |
| query                   | 查询参数                         | Object   | sort:\['custom','asc'\] (desc:降序，asc:  升序) | {sort::\['id','desc'\]}                                          |
| params                  | 查询参数                         | Object   | \---                                            | {}                                                               |
| form                    | 查询表单对象                     | Object   | \---                                            | {}                                                               |
| defaultForm             | 重置参数                         | Function | \---                                            | ()=>{}                                                           |
| time                    | 更新表格数据时显示延迟时间(ms)   | Number   | \---                                            | 50                                                               |
| crudMethod              | crud  方法                       | Object   | add:()=>{},del:()=>{},edit:()=>{},get:()=>{}    | {add:()=>{},del:()=>{},edit:()=>{},get:()=>{}}                   |
| optShow                 | 主页操作栏显示按钮对象           | Object   | add,sel,edit,del,download,reset                 | {add:true,sel:false,edit:true,del:true,download:true,reset:true} |
| props                   | 自定义拓展属性                   | Object   | \---                                            | {}                                                               |
| queryOnPresenterCreated | 是否在初始化时显示数据           | Boolean  | \---                                            | true                                                             |
| debug                   | 是否开启调试                     | Boolean  | \---                                            | false                                                            |

### Default Attribute Description

| **属性名**      | **说明**              | **默认值**                                                            |
| --------------- | --------------------- | --------------------------------------------------------------------- |
| dataStatus      | 记录数据状态          | {}                                                                    |
| status          | 操作状态，同  optShow | add:NORMAL，edit:NORMAL，cu，title                                    |
| msg             | 操作提示              | {submit: '提交成功',add: '新增成功',edit: '编辑成功',del: '删除成功'} |
| page            | 分页对象              | { page:0,//  页码 size:10,//每页数据条数 total:0,//总数据条数 }       |
| loading         | 表格加载状态          | false                                                                 |
| downloadLoading | 表格导出状态          | false                                                                 |
| delAllLoading   | 表格项删除状态        | false                                                                 |

### Constant Description

#### CRUD STATUS

| **状态名** | **值** | **状态描述** |
| ---------- | ------ | ------------ |
| NORMAL     | 0      | 初始状态     |
| PREPARED   | 1      | 已完成       |
| PROCESSING | 2      | 进行中       |

#### CRUD METHODS

| **方法名**             | **描述**                                                                                        |
| ---------------------- | ----------------------------------------------------------------------------------------------- |
| submitSuccessNotify    | 提交成功的提示                                                                                  |
| addSuccessNotify       | 新增成功的提示                                                                                  |
| editSuccessNotify      | 编辑成功的提示                                                                                  |
| delSuccessNotify       | 删除成功的提示                                                                                  |
| toQuery                | 查询/初始化数据                                                                                 |
| refresh                | 刷新组件/更新数据                                                                               |
| toAdd                  | 开启添加                                                                                        |
| toEdit                 | 开启编辑                                                                                        |
| toDelete               | 开启删除                                                                                        |
| cancelDelete           | 取消删除                                                                                        |
| cancelCU               | 取消新增/编辑                                                                                   |
| submitCU               | 提交新增/编辑                                                                                   |
| doAdd                  | 执行添加                                                                                        |
| doEdit                 | 执行编辑                                                                                        |
| doDelete               | 执行删除                                                                                        |
| doExport               | 通用导出                                                                                        |
| getQueryParams         | 获取查询参数                                                                                    |
| pageChangeHandler      | 分页-当前页改变                                                                                 |
| sizeChangeHandler      | 分页-每页条数改变                                                                               |
| dleChangePage          | 预防删除第二页最后一条数据时，或者多选删除第二页的数据时，页码错误导致请求无数据                |
| selectionChangeHandler | 多选表格-选择改变                                                                               |
| resetQuery             | 重置查询参数                                                                                    |
| resetForm              | 重置表单                                                                                        |
| resetDataStatus        | 重置数据状态                                                                                    |
| getDataStatus          | 获取数据状态                                                                                    |
| selectAllChange        | 用于树形表格多选,  选中所有                                                                     |
| selectChange           | 用于树形表格多选，单选的封装                                                                    |
| toggleRowSelection     | 切换选中状态                                                                                    |
| findVM                 | 查找组件示例                                                                                    |
| notify                 | 通知提示，同[Element Notification](https://element.eleme.cn/#/zh-CN/component/notification)通知 |
| updateProp             | 更新自定义拓展属性                                                                              |
| getDataId              | 获取数据的  id                                                                                  |
| getTable               | 获取表格的实例                                                                                  |
| attchTable             | 树形数据表格                                                                                    |

#### CRUD HOOK

| **函数名**         | **值**                 | **描述**                 |
| ------------------ | ---------------------- | ------------------------ |
| beforeRefresh      | beforeCrudRefresh      | 刷新之前                 |
| afterRefresh       | afterCrudRefresh       | 刷新之后                 |
| beforeDelete       | beforeCrudDelete       | 删除之前                 |
| afterDelete        | afterCrudDelete        | 删除之后                 |
| beforeDeleteCancel | beforeCrudDeleteCancel | 删除取消之前             |
| afterDeleteCancel  | afterCrudDeleteCancel  | 删除取消之后             |
| beforeToAdd        | beforeCrudToAdd        | 新建之前                 |
| afterToAdd         | afterCrudToAdd         | 新建之后                 |
| beforeToEdit       | beforeCrudToEdit       | 编辑之前                 |
| afterToEdit        | afterCrudToEdit        | 编辑之后                 |
| beforeToCU         | beforeCrudToCU         | 开始  "新建/编辑"  之前  |
| afterToCU          | afterCrudToCU          | 开始  "新建/编辑"  之后  |
| beforeValidateCU   | beforeCrudValidateCU   | "新建/编辑"  验证   之前 |
| afterValidateCU    | afterCrudValidateCU    | "新建/编辑"  验证   之后 |
| beforeAddCancel    | beforeCrudAddCancel    | 添加取消之前             |
| afterAddCancel     | afterCrudAddCancel     | 添加取消之后             |
| beforeEditCancel   | beforeCrudEditCancel   | 编辑取消之前             |
| afterEditCancel    | afterCrudEditCancel    | 编辑取消之后             |
| beforeSubmit       | beforeCrudSubmitCU     | 提交之前                 |
| afterSubmit        | afterCrudSubmitCU      | 提交之后                 |
| afterAddError      | afterCrudAddError      | 新建失败之后             |
| afterEditError     | afterCrudEditError     | 编辑失败之后             |

    <script>
    export default {
      methods: {
        [CRUD.HOOK.hook_name](crud, form) {}
      }
    }
    </script>

#### CRUD NOTIFICATION_TYPE

| **属性名** | **属性值** | **类型** |
| ---------- | ---------- | -------- |
| SUCCESS    | success    | 成功     |
| WARNING    | warning    | 警告     |
| INFO       | info       | 信息     |
| ERROR      | error      | 错误     |
