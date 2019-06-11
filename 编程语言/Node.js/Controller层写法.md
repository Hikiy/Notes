# Controller层写法
app/controller/hikitest_controller:
```
'use strict';

const Controller = require('egg').Controller;
const Output = require('../helper/output_helper');

class HikitestController extends Controller{
    async gethikiname(){
        const { ctx } = this; 
        //const ctx = this.ctx;

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
**注意末尾需要输出**

<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki  
> 创建日期：2019.04.30   
> 更新日期：2019.04.30

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  