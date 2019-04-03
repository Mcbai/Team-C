 # 接口管理与Mock
### 1. 接口管理
在前后端分离开发的过程中，数据接口是程序中前后端的一个主要交互流转过程，接口管理的工程化程度直接影响了前后端联合开发的质量以及之后联调的效率。
为了业务项目开发的进一步前后端切分以及增加联调环节的效率，避免一些不必要的沟通协作问题。现在列出一些接口管理的约定建议。

1. 接口统一生成文档在统一平台管理。
2. 需在开发前商定好接口参数以及文档，前后端严格按照文档进行接口实现
3. 文档应给出一组或者多组示例数据，供前端mock使用，避免后端接口进度影响
4. 接口变动时应同步提醒相关的前后端开发人员，保持信息的时效性

#### 对于前端的接口使用方面
1. 严格按照文档进行接口mock，保证请求方式及参数正确
2. 切记在前后端联调时删除业务文件中的mock部分
3. 可编写对于接口的测试脚本，使用自动化工具以及测试用例进行一键测试，帮助减少后端接口问题
4. 测试联调时出现问题应与文档对照，确保与文档一致无误后再与后端沟通接口异常问题

>参考：阿里云API网关功能，可参考其功能逐渐完善api相关功能

### 2.数据Mock部分
前端工程中可使用[Mock.js](http://mockjs.com/)进行后端接口mock，这样可以使前端可以独立于后端开发单独进行开发，提高了开发效率。
使用Mock.js可以在不修改既有代码的情况进行Ajax请求拦截，返回预定好的数据。同时使用方法符合与平常接口调用，返回数据可使用内置函数进行随机数据模拟，更接近于真实数据，方便单元测试。
使用方法:
1. 安装Mock.js
`npm install mockjs --save`
2. 在业务组件需要Mock的文件中进行调用
```javascript
/** mock begin */
import Mock from 'mockjs'
const Random = Mock.Random
Mock.mock(/\/api\/demo/, 'post', options => {
    return {
        'content': {
            'data': {
                'name': Random.cname(),
                'id': Random.id(),
                'isTeacher': Random.boolean(),
                'create-time': Random.date('yyyy-MM-dd'),
                'color': Random.color(),
                'desc': Random.csentence(),
                'blog': Random.url('http', 'xshcs.com'),
                'hometown': Random.county(true),
                'classId': JSON.parse(options.body).lessonCode
            }
        },
        'code': 0,
        'msg': ''
    }
})
/** mock end */
```
3. 根据业务需求更改mock正则匹配表达式，请求方式以及返回数据(模拟数据数据见[官网示例](http://mockjs.com/examples.html))
4. 前后端进行联调时删去mock注释中的内容即可

> 相应代码生成已经整合进[code-snippets](../example/mock.code-snippets)中（输入create-mock直接调用）
