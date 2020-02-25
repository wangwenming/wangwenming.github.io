https://blog.csdn.net/fygu18/article/details/81049852

1.最上层controller和TService是我们阿里分层规范里面的第一层:轻业务逻辑，参数校验，异常兜底。通常这种接口可以轻易更换接口类型,所以业务逻辑必须要轻，甚至不做具体逻辑。

2.Service：业务层，复用性较低，这里推荐每一个controller方法都得对应一个service,不要把业务编排放在controller中去做，为什么呢？如果我们把业务编排放在controller层去做的话，如果以后我们要接入thrift,我们这里又需要把业务编排在做一次，这样会导致我们每接入一个入口层这个代码都得重新复制一份如下图所示: 


DO（Data Object）：与数据库表结构一一对应，通过DAO层向上传输数据源对象。 

DTO（Data Transfer Object）：数据传输对象，Service或Manager向外传输的对象。 

BO（Business Object）：业务对象。由Service层输出的封装业务逻辑的对象。 

AO（Application Object）：应用对象。在Web层与Service层之间抽象的复用对象模型，极为贴近展示层，复用度不高。 

VO（View Object）：显示层对象，通常是Web向模板渲染引擎层传输的对象。 

Query：数据查询对象，各层接收上层的查询请求。注意超过2个参数的查询封装，禁止使用Map类来传输。

Integer id, AgencyRequest agencyRequest


WebankCreateAgencyRequest request

curl -X POST -H"Content-Type: application/json" --data '{"merOrderNo":"123", "name": "孙白雪", "idType": "01", "idNo": "232301199803191321"}' http://localhost:12509/api/agency

