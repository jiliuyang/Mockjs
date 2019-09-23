# vue-cli项目中mockjs使用说明

## 前序

当前项目开发工作的主流方式是前后端分离，这样在某种程度上可以使前后端各司其职，提高效率。但在项目起步阶段，后端接口尚未提供，而前端在先行开发界面时很多时候需要模拟一些假数据以便于展示，走通流程。我想大部分前端都尝试过先配置一个json文件，填充一些假数据进去来调用模拟后台返回数据。当然，这种造数据的方式是很枯燥的，数据也不够灵活。现在我们有了更好的选择，就是mockjs.

mockjs最好用的地方就在于它能够拦截ajax请求，并能更加方便的为你动态模拟假数据。

## 准备工作

vue-cli项目搭建完成后，需先行安装axios和mockjs
```
npm install axios --save
npm install mockjs --save-dev
```
在项目目录中新增mock/index.js路径文件，用以配置mock模拟数据的格式等,接下来先示范一下基础用法：
```
import Mock from "mockjs";
const Random = Mock.Random;  //Mock.Random是一个工具类，用于产生各种随机数据


const usualTemplate = {
  status: 200,
  data: {
    message: "基础信息模板展示"
  }
}

Mock.setup({
  timeout: '500'
})

Mock.mock('/user/list', usualTemplate);


export default Mock;
```
然后在main.js文件中引入mockjs
```
......
import "./mock"
......
```
到此时，mockjs的安装及配置等准备工作已完成。

## mock语法简述

在index.js文件中，我们在引入mockjs后，有使用Mock.mock()方法，该方法用以设置模拟数据的数据模板，基础语法即：
```
Mock.mock(url,template)
```
其中template模板可以是一个对象或字符串,url是要拦截的ajax请求地址，只要匹配到对应的url字符串即可以进行拦截调用。如下示例
```
import Mock from "mockjs";
const Random = Mock.Random;  //Mock.Random是一个工具类，用于产生各种随机数据


const usualTemplate = {
  status: 200,
  data: {
    message: "基础信息模板展示"
  }
}

Mock.setup({
  timeout: '500'
})

Mock.mock('/user/list', usualTemplate);


export default Mock;
```
在任意一个视图组件中进行如下调用即可：
```
<script>
import axios from "axios";
export default {
  data() {
    return {};
  },
  mounted() {
    this.testMock1();
  },
  methods: {
    testMock1() {
      axios.post("/user/list").then(res => {
        console.log(res);
      });
    }
  }
};
```
在控制台打印出返回的结果如下图所示:

![mock返回结果](/img/result.png)

我们可以看到，mock返回了一套比较规范的数据返回模板，当然仅仅是这样的话，与我们之前采用json模板没多大区别，反而要引入配置似乎又显得复杂了。


## mock.Random工具类
其实，mock的便利当然不止这些，里面最常用且最方便的是其提供的一个Mock.Random工具类，它可以用于生成各种数据。

常见的随机数据如下：
```
Mock.mock({

            //basic
            'boolean': Random.boolean(), // 返回一个随机的布尔值。
            'natural': Random.natural(1, 100), // 返回一个随机的自然数（大于等于 0 的整数）
            'integer': Random.integer(1, 100), // 生成1到100之间的整数
            'float': Random.float(0, 100, 0, 5), // 生成0到100之间的浮点数,小数点后尾数为0到5位
            'character': Random.character(), // 生成随机字符,可加参数定义规则
            'string': Random.string('壹贰叁肆伍陆柒捌玖拾', 3, 5),//返回一个随机字符串。
            'range': Random.range(0, 10, 2), // 生成一个随机数组

            //date
            'date': Random.date('yyyy-MM-dd'), // 生成一个随机日期,可加参数定义日期格式
            'time': Random.time('HH:mm:ss'), //获取一个随机时间
            'datetime': Random.datetime(), // 返回一个随机的日期和时间字符串。
            'now': Random.now(), // 返回当前的日期和时间字符串。

            //image
            'image': Random.image('200x100', '#00405d', '#FFF', 'Mock.js'),//生成一个随机的图片地址。
            'dataImage': Random.dataImage(Random.size, 'hello'),//生成一段随机的 Base64 图片编码。

            //color
            'color': Random.color(),//随机生成一个有吸引力的颜色，格式为 '#RRGGBB'。
            'hex': Random.hex(), //随机生成一个有吸引力的颜色，格式为 '#RRGGBB'。
            'rgb': Random.rgb(), //随机生成一个有吸引力的颜色，格式为 'rgb(r, g, b)'。
            'rgba': Random.rgba(), //随机生成一个有吸引力的颜色，格式为 'rgba(r, g, b, a)'。
            'hsl': Random.hsl(), //随机生成一个有吸引力的颜色，格式为 'hsl(h, s, l)'。

            //text
            'paragraph': Random.paragraph(3, 7), // 随机生成一段文本。
            'cparagraph': Random.cparagraph(1, 3), // 随机生成一段中文文本。
            'sentence': Random.sentence(1, 3), // 随机生成一个句子，第一个单词的首字母大写。
            'csentence': Random.csentence(1, 3),// 随机生成一段中文文本。
            'word': Random.word(1, 3),// 随机生成一个单词。
            'cword': Random.cword('零一二三四五六七八九十', 10, 15),//生成中文10到15个
            'title': Random.title(3, 5), // 随机生成一句标题，其中每个单词的首字母大写。
            'ctitle': Random.ctitle(3, 7), // 随机生成一句中文标题。

            //name
            'first': Random.first(),// 随机生成一个常见的英文名。
            'last': Random.last(),// 随机生成一个常见的英文姓。
            'name': Random.name(),// 随机生成一个常见的英文姓名。
            'cfirst': Random.cfirst(),// 随机生成一个常见的中文名。
            'clast': Random.clast(), // 随机生成一个常见的中文姓。
            'cname': Random.cname(),//随机生成一个常见的中文姓名。

            //web
            'url': Random.url('http', 'baidu.com'), // 随机生成一个 URL。
            'protocol': Random.protocol(), //随机生成一个 URL 协议
            'domain': Random.domain(), //随机生成一个域名。
            'tld': Random.tld(), //随机生成一个顶级域名
            'email': Random.email('qq.com'),//随机生成一个邮箱
            'ip': Random.ip(),//随机生成一个 IP 地址。

            //address
            'region': Random.region(),//随机生成一个（中国）大区。
            'province': Random.province(),//随机生成一个（中国）省（或直辖市、自治区、特别行政区）
            'city': Random.city(true),//布尔值。指示是否生成所属的省。
            'county': Random.county(true),//随机生成一个（中国）县。
            'zip': Random.zip(),//随机生成一个邮政编码（六位数字）
            'address': Random.province(), // 生成地址

            //helper
            'capitalize': Random.capitalize('hello'),//把字符串的第一个字母转换为大写。
            'upper': Random.upper('hello'),//把字符串转换为大写。
            'lower': Random.lower('HELLO'),//把字符串转换为小写。
            'pick': Random.pick(['a', 'e', 'i', 'o', 'u']),//从数组中随机选取一个元素，并返回。
            'shuffle': Random.shuffle(['a', 'e', 'i', 'o', 'u']), //打乱数组中元素的顺序，并返回。

            //miscellaneous
            'guid': Random.guid(), //随机生成一个 GUID。
            'id': Random.id(), //随机生成一个 18 位身份证。
            'increment': Random.increment(2), //生成一个全局的自增整数。自增为2
        })
```

我们可以借助这个工具类，产生我们所需要的差不多一切数据，免除了自己制造假数据又想让假数据尽量逼真的枯燥过程。

## Mock.setup(settings)

另外我们提一下setup这个方法，为了使模拟地址拦截数据回调的效果更加逼真，可以通过该方法指定被拦截的ajax请求的拦截时间，单位是毫秒。值可以是正整数，也可以是以斜杠分隔的一个区间。如：
```
Mock.setup({ timeout : 400})
Mock.setup({ timeout : '200-600' })
```

## 配置过程中所遇到的坑
在配置和使用mock过程中，url地址的配置在官网中是支持使用正则的，之前在vue项目中也正常使用过，但后面自己在写这篇文章的时候使用例子测试时，使用正则来进行地址匹配
```
Mock.mock('/\/user\/list/', usualTemplate)
```
控制台会报404的错误，最后去掉正则直接以地址的部分字符串做匹配，则可以正常调用，暂时未找出原因，怀疑是vscode的某些配置原因。


## 最后给出一个常用的返回table数据的生成模板样例：
```
import Mock from 'mockjs' // 引入mockjs
const Random = Mock.Random // Mock.Random 是一个工具类，用于生成各种随机数据

// 模拟获取订单列表
export const order_list = (params) => {
  let list = []
  doCustomTimes(10, () => {
    list.push(Mock.mock({
      // basic
      'orderId': Random.integer(1, 1000000000),
      'userName': Random.cname(), // 随机生成一个常见的中文姓名。
      'idCard': Random.id(), // 随机生成一个 18 位身份证。
      'contractId': 'YZ-0101-JFGD-201904001',
      'carType': Random.pick(['2017款 朗逸 1.6L 自动风尚版', '2018款 朗逸 1.6L 自动夏季版', '2019款 朗逸 1.6L 自动风尚版']),
      'frameNumber': Random.pick(['JF1SH52F69G038254', 'JF1SH52F69G038679', 'JF1SH52F69G036682']),
      'rentalCapital': Random.float(0, 100000, 0, 2)
    }))
  })
  return {
    total: 86,
    data: list
  }
}

/**
 * @param {Number} times 回调函数需要执行的次数
 * @param {Function} callback 回调函数
 */
export const doCustomTimes = (times, callback) => {
  let i = -1
  while (++i < times) {
    callback(i)
  }
}
```