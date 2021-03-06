# 进阶

## 高级类型
### 图片类型 && 富文本类型 && 关联类型
现在要做一个商品上架功能，操作跟上一章一样
```js
 {
  id: { type: 'INTEGER', primaryKey: true, autoIncrement: true, comment: '商品ID' },
  title: { type: 'STRING(50)', comment: '商品名称' },
  category: {
    type: 'relate',
    relation: [
      { label: '服装', value: 1 },
      { label: '食品', value: 2 },
      { label: '日常用品', value: 3 },
      { label: '电子产品', value: 4 },
      { label: '书籍', value: 5 }
    ],
    comment: '商品分类'
  },
  img: { type: 'img', comment: '商品图片' },
  detail: { type: 'richtext(3000)', comment: '商品详情' },
  storeNumber: { type: 'INTEGER', comment: '库存' },
  status: {
    type: 'relate',
    relation: [
      { label: '上架', value: 1 },
      { label: '下架', value: 2 },
      { label: '售罄', value: 3 }
    ],
    comment: '状态'
  },
  created_at: { type: 'DATE', comment: '创建时间' },
  updated_at: { type: 'DATE', comment: '更新时间' }
}
```
为了保留状态，这次我们可以通过项目提供的接口``/localhost:7001/api/code/produce``进行代码生成，使用**postman**进行接口提交
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502154916.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502154916.png)</a>
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502154945.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502154945.png)</a>

:::tip
这个接口还提供了一个参数icon用于设置菜单的图标，支持的图标可以查看[antd-design图标文档](https://ant.design/components/icon-cn/)
:::

点击提交发送参数到接口
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502160244.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502160244.png)</a>
接口返回代码生成成功，这时候前台和后台又会检测到代码变化，重新开始编译，编译完记得运行``npx sequelize db:migrate``进行数据表的生成哦
```shell
npx sequelize db:migrate

Sequelize CLI [Node: 10.16.0, CLI: 5.5.1, ORM: 4.44.4]

Loaded configuration file "database/config.json".
Using environment "development".
sequelize deprecated String based operators are now deprecated. Please use Symbol based operators for better security, read more at http://docs.sequelizejs.com/manual/tutorial/querying.html#operators node_modules/.4.44.4@sequelize/lib/sequelize.js:245:13
== goods: migrating =======
== goods: migrated (0.204s)
```
之后就是点击生成的菜单**商品管理**进行增删改查的测试啦
- 关联类型
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165408.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165408.png)</a>
- 图片类型 & 富文本类型
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165458.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165458.png)</a>
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165627.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165627.png)</a>
点击确定，新增成功！
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165848.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502165848.png)</a>

编辑和删除和上一章操作一样，这里不再赘述。

## 生成接口给客户端（如你的商城APP、小程序等)使用
每次你生成一次代码，都会在egg的路由中生成一套restful风格的接口。

你可以通过这些接口在你的客户端中进行增删改查，如果你觉得这些功能还不能满足你的需求（比如一些联表查询操作等),你可以通过学习egg开发找到对应的源代码进行修改😉

比如上面的商品管理功能会生成如下的接口
| 请求方式        | 接口路径          | 接口功能 |
| :-------------: |:-------------:| :-----:|
| GET   |  /goods | 获取所有商品数据|
| GET |  /goods/:id      |    获取单个商品数据 |
| POST |  /goods      |   新增商品 |
| PUT | /goods/:id      |    修改单个商品数据 |
| DELETE |goods/:id      |    删除单个商品 |

>具体实现可以查看[egg restful风格路由](https://eggjs.org/zh-cn/basics/router.html#restful-%E9%A3%8E%E6%A0%BC%E7%9A%84-url-%E5%AE%9A%E4%B9%89)

## 登录普通权限的账号查看效果
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502173005.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502173005.png)</a>
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502173114.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502173114.png)</a>
<a data-fancybox title="" href="https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502173207.png">![](https://cdn.jsdelivr.net/gh/jsyang666/imgbed/20200502173207.png)</a>
可以看到，使用我们在管理员管理的账号密码登录后只能看到**学生管理**和**商品管理**两个菜单，并没有**管理员管理**和**代码生成**两个菜单权限，这样就可以将这个账号密码给你的客户使用了。
:::danger 警告
当然最好不要将代码中具有超级管路员权限的账号admin密码jsyang666直接部署到你的应用中，你应该自己去修改代码，以免具有超级管理员权限的账号密码被坏人使用，造成损失。作者接下来也会努力在权限方面进行开发迭代，以降低这个风险。
:::

首次登陆你可能会进入这个页面
<a data-fancybox title="" href="https://gitee.com/hyy930256283/imgbed/raw/master/blog/20200502175120.png">![](https://gitee.com/hyy930256283/imgbed/raw/master/blog/20200502175120.png)</a>
那是因为首页设置默认路由是``http://localhost:8000/#/manage/admin``,而这个路由只有超级管理员才有权限访问，所以会显示403,你可以去antd-pro的路由配置更改默认路由为``http://localhost:8000/#/manage/student``,这样你的客户刚登陆进来就不会一头雾水了。
```js
    routes: [
// ---        { path: '/', redirect: '/manage/admin' },
+++        { path: '/', redirect: '/manage/student' },
```


如果你的项目通过JsyangAdmin开发完了，可以看下一章[构建与部署](/build)学习如何将这个项目部署到服务器中，让你的用户通过网络访问你的应用。
