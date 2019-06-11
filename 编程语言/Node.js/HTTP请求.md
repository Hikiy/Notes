# HTTP请求
### GET请求
- Controller层
```
'use strict';

const Controller = require('egg').Controller;
const Output = require('../helper/output_helper');

class HikitestController extends Controller{
    async gethikiname(){
        // const { ctx } = this; 
        const ctx = this.ctx;

        let name = ctx.query.name;
        let hikiname = 'hiki';

        if( name != undefined ){
            hikiname = 'no,his name is hiki';
        }

        ctx.body = Output.success_return(hikiname);
    }
}

module.exports = HikitestController;
```
- 路由配置
```
'use strict';

/**
 * @param {Egg.Application} app - egg application
 */
module.exports = app => {
    const { router, controller } = app;
    router.get('/gethikiname', controller.hikitestController.gethikiname);
};
```
### POST请求
- Controller层
```
'use strict';

const Controller = require('egg').Controller;
const Output = require('../helper/output_helper');

class HikitestController extends Controller{
    async gethikiname(){
        // const { ctx } = this; 
        const ctx = this.ctx;

        let name = ctx.request.body.name;
        let hikiname = 'hiki';

        if( name != undefined ){
            hikiname = 'no,his name is hiki';
        }

        ctx.body = Output.success_return(hikiname);
    }
}

module.exports = HikitestController;
```
- 路由配置
```
'use strict';

/**
 * @param {Egg.Application} app - egg application
 */
module.exports = app => {
    const { router, controller } = app;
    router.post('/gethikiname', controller.hikitestController.gethikiname);
};
```
- 然后测试报错
```
`invalid csrf token`
```
- 原因是egg这个框架在安全方面做了处理，要禁用csrf以测试
- 在config.default.js 中禁用 csrf 
```
config.security = {
        csrf:{
            enable:false
        }
    }
```
- 然后用postman发送post请求发现post传过去的值为undefined
- 需要使用另一个格式传值，使用x-www-form-urlencoded传值，或者说headers加上`[{"key":"Content-Type","value":"application/x-www-form-urlencoded"}]`
- 成功

<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki  
> 创建日期：2019.04.30  
> 更新日期：2019.04.30

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  