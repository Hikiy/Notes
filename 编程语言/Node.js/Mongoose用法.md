# Mongoose用法
### model层：
```
'use strict';

module.exports = app => {
    const mongoose = app.mongoose;
    const Schema = mongoose.Schema;

    const ExpressSchema = new Schema({
        express_code: { type: String },
        express_no: { type: String },
        mobile: { type: String },
        express_route: { type: Array },
        created: { type: Number, default: Math.round(new Date().getTime()/1000) },
        updated: { type: Number, default: Math.round(new Date().getTime()/1000) },
    }, {
        collection: 'EXPRESS' //集合名
    });

    ExpressSchema.index({ express_no: -1 });    //索引，1为正序 -1为倒序
    ExpressSchema.index({ created: 1 });
    ExpressSchema.index({ updated: -1 });

    return mongoose.model('ExpressModel', ExpressSchema);
}
```
- `Schema` ： 一种以文件形式存储的数据库模型骨架，不具备数据库的操作能力
- `Model` ： 由Schema发布生成的模型，具有抽象属性和行为的数据库操作对
- `Entity` ： 由Model创建的实体，他的操作也会影响数据库  

`Schema`生成`Model`，`Model`创造`Entity`，`Model`和`Entity`都可对数据库操作造成影响，但`Model`比`Entity`更具操作性。

### service层:
- 插入数据
```
expressInfo = {
    express_code: expressCode,
    express_name: expressName,
    express_no: expressNo,
    express_route: [ {
        content: '暂无物流信息',
        time: Moment().format('YYYY-MM-DD HH:mm:ss')
    } ]
};
let expressM = this.ctx.model.ExpressModel();
expressM.express_code = expressInfo.express_code;
expressM.express_no = expressInfo.express_no;
expressM.mobile = mobile;
expressM.express_route = [];
expressM.save();
```
- 查询一条记录
```
let expressQuery = await this.ctx.model.ExpressModel.findOne({
                express_code: expressCode,
                express_no: expressNo
            }).exec();
```

<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki  
> 创建日期：2019.05.05  
> 更新日期：2019.05.05

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  