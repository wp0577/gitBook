# web开发三层架构

web开发三层架构分为web层，service层，dao层。

service层没有复杂的数据提取业务，大部分的数据业务处理在dao层进行。

web层主要作为接受浏览器请求，并返回请求。web层与service层进行直接沟通，没有与dao层的联系。

