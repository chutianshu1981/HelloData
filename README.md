HelloData分析：
1、HelloData.FrameWork:为数据库底层框架，支持多种数据库操作，加入了BaseEntity与BaseLogic，BaseManager两个有关业务逻辑的继承方式。在数据库生成model的时候使用T4生成，
并且生成的数据库表对应的对象类为部分类（partial），如果需要扩展加入当前对象的多个部分类即可。这样做的好处是将数据库生成
的类与业务间的操作分离。BaseLogic的继承BaseLogic<T>，T为操作数据库表对象的泛型，里面包含的常用的新增，删除，修改，获取一个实体，获取实体list
,BaseManager<T, TU>,T为操作逻辑类，TU为操作逻辑对象类。继承后当前操作逻辑类为全局唯一实例，使用了单一模式，操作方法也是包含了那些常用的逻辑操作。
书写Demo:

 using (DeleteAction delete = new DeleteAction(Entity))
 {
     delete.SqlWhere(cms_user.Columns.id, "1,2,3,4,5", RelationEnum.In);
     delete.Excute();
     return delete.ReturnCode;
 }
 
using (UpdateAction update = new UpdateAction(Entity))
 {
     update.SqlKeyValue(cms_user.Columns.createtime, null);
     update.SqlKeyValue(cms_user.Columns.password, "123456123");
     update.Excute();
     return update.ReturnCode;
 }
 
using (SelectAction select = new SelectAction(Entity))
 {
     if (!string.IsNullOrEmpty(username))
         select.SqlWhere(cms_user.Columns.username, username, RelationEnum.Like, ConditionEnum.Or);
     select.SqlPageParms(pageSize);
     return select.QueryPage<cms_user>(pageIndex);
 }
 
using (SelectAction action = new SelectAction(""))
 {
    
     action.SqlWhere(cms_user.Columns.username, "admin");
     action.SqlWhere(cms_user.Columns.password, "123456");
     PageList<cms_user> lists= action.QueryPage<cms_user>(1);
     return null;
 }
 操作数据库的时候可以加入缓存，缓存现支持webcache,Redis，MemberCache ，后两种可以支持分布式部署操作；
 2、HelloData.FWCommon：包含加密解密；导出操作：txt,csv，excel；序列化与反序列化：二进制，json，soap,xml；
 其他的常用操作，例如：html操作，socket网络爬虫等。
 3、HelloData.FWExtend：这个为开发人员项目操作的，基于HelloData.FrameWork的扩展；
 4、HelloData.Web：里面加入url重写，多语言模块，ajax请求类似mvc的操作。
 操作demo:
 
        function ajaxdemo() {
            $.ajax({
                type: 'POST',
                url: "ajax/demo/do",
                data: "{  'result':{ 'Result':-1,'Message':'不支持GET请求','PostTime':'2012-2-2'},'ido':233}",
                contentType: "application/json",
                dataType: "json"
            });
        }
