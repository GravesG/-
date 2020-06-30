> mybatis 的缺点

- 可移植性差，sql依赖数据库，切库可能会有影响



> mybaits 的核心组件

- SqlSessionFactoryBuilder:根据配置信息或者代码生成Sql'SessionFactory
- SqlSessionFactory: 依靠工厂来生成SqlSession
- SqlSession： 是一个既可以发送sql去执行返回结果，也可以获取Mapper接口 。
- SqlMapper： 由一个Java接口和XMl文件构成的，需要给出对象的sql和映射关系



> SqlSessionFactoryBulider 的生命周期

利用xml或者java代码来构建sqlSessionFacetoryBulider,它可以创建多个sessionFactroy，一旦构建成功sessionFactory，就可以回收他了。



