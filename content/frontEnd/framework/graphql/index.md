### 一、GraphQL介绍
 GraphQL是facebook开发的一种 ‘API的查询语言’ ，于2015年公开发布，是REST API的替换品。  
 GraphQL即是一种用于 API的查询语言，也是一个满足你数据查询的运行时，它对你的API中的数据提供了一套易于理解的完整描述，使得客户端能够准确获得它需要的数据，而且没有任何冗余，也让API更容易地随着实际推移而演进，还能用于构建强大的开发者工具。  
 GraphQL通常被称作声明式数据获取语言，其设计原则：层次结构性、以产品为中心、强类型、客户端定制查询，内省。

 官网：http://graphql.org  
 中文网：http://graphql.cn  
#### 1、特点
- 请求你所要的数据，不多不少  
  可以根据自己所需要的字段，去定制获取所需要的数据，比如'hero'中有'name','age','sex'等，可以只取得需要的字段

- 获取多个资源，只用一个请求  
  典型的REST API请求多个资源时得载入多个URL，而GraphQL可以通过一次请求就可以获取你应用所需的所有数据。这样也能保证在较慢的移动网络连接下，使用GraphQL的应用也能表现得足够迅速。

- 描述所有可能类型的系统，便于维护，根据需求平滑演进，添加或隐藏字段  
  GraphQL使用类型来保证应用只请求可能的数据，还提供了清晰的辅助性错误信息，应用可以使用类型，而避免编写手动解析代码

#### 2、GraphQL与restful对比
- ‘restful’全称‘Representational State Transfer’表属性状态转移。本质上就是定义uri,通过API接口来取得资源，通过用系统架构，不受语言限制。比如：'restapi/shopping/v3/restauants?id=13'就是典型的restful接口，定义资源+查询条件。
- ‘restful’一个接口只能返回一个资源，GraphQL用类型区分资源

### 二、涉及到的主要模块
- graphql:解析graphql query
 #### 1、前端 
 - apollo-boost:包含我们认为对应构建Apollo应用程序至关重要的软件包，例如内存缓存，本地状态管理和错误处理，还具有足够灵活性来处理身份验证等功能，这包括以下模块：
 - apollo-client:所有魔法发生的地方
 - apollo-cache-inmemory:推荐的缓存
 - apollo-link-http:用于远程数据获取的apollo链接
 - apollo-link-error:用于错误处理的apollo链接
 - apollo-link-state：用于管理state的apollo link
 - graphql-tag:导出query和mutation的gql函数
 - graphql-test-utils
 - graphql-tools
 - react-apollo:集成react视图，其中有ApolloProvider与react的上下文提供程序类似，包装了react应用晨曦并将client放在上下文中，允许你从组件树种的任何位置访问它

##### 2、后端
- graphql-servre-core
- graphql-server-express
- graphql-server-module-graphiql
- graphql-subscriptions
- graphql-tools:通过makeExecutableSchema创建schema不仅包含了类型的定义，而且包含了解析函数，行为动态构建和修改schema.
- subscriptions-transport-ws




