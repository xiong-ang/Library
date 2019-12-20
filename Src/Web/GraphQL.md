# GraphQL
> 相信大家都接触过前后端交互的场景，不知道是否想过一个问题，前后端的交互，或者说广义的交互，本质上处理的对象都是**数据**，而软件长期发展过程中，建立起来的比较完善的**数据交互模型**之一就是：针对数据集的**CURD接口**。通过CURD接口，数据提供方和数据请求方不再需要约定严格和数量众多的交互接口，数据请求方有更多的自主。     
> 就以Rest API为例，通常情况下是后端提供了什么样的接口，前端才能获取什么样的数据，这种模式虽然简单，但是容易引起的问题：   
        1. 数据冗余，针对需要某个请求数据的部分数据的场景     
        2. API数量庞杂，针对不同的client，如果存在细微的差别，仍然需要提供新的API接口   
        3. 应对需求乏力，针对新需求，需要提供新接口       
> 如果能够通过某种机制，将客户需要的所有数据都暴露出去，而客户端可以**按需进行CURD**，那前后端交互的场景会简化很多，而这就是GraphQL提供的价值。   
> 本文将围绕GraphQL，介绍其基本知识，核心概念，生态圈以及基于Node的实战总结。     

## 什么是GraphQL
什么是GraphQL，回到本质，GraphQL处理数据交互的问题，GraphQL定义了一套**查询语言标准**。而GraphQL的实现，则是一套**运行时**，通过它，可以对基于GraphQL定义的数据类型系统进行CURD。   
与数据库CURD不同的地方是，多数情况下，GraphQL连接的对象是数据和操作(Service层)，而不是数据库。     

## GraphQL核心概念    
### Types and Fields       
Type和Field是GraphQL提供的最重要的概念，如果需要创建GraphQL服务，需要首先创建Type，每个Type有一个或多个Field组成，然后需要为每个Type的每个字段提供响应的处理函数。以NodeJS为例：
```javascript
// 通过字符串的方式定义Type和Field
var schema = buildSchema(`
  type Query {
    hello: String
  }
`);

// 为每个Field定义相应的处理函数
var root = { hello: () => 'Hello world!' };

// 创建express中间件
var graphqlMiddleware = graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
});
```

这样GraphQL客户端就可以对相应的Field进行查询操作：       
```GraphQL
Query {
  hello
}
```   

### Queries and Mutations         
GraphQL最重要的两个Type是Query(查询)和Mutation(修改)，而且这两个Type是其他所有Type的入口，也就是说所有对Field的操作都是从Query和Mutation出发，找到该字段，从而进行操作。Query与Mutation的概念可以类比Http中Get与Post的关系：可以在Query的处理函数中进行数据的改写，但是一般约定Query用于获取数据，Mutation用于修改数据。           
还是以express-graphql为例来说明Query和Mutation的功能：          
```javascript   
// 定义schema，总共五个Field：getMessage，createMessage以及Message里面的三个
// MessageInput为输入类型，Mutation的结构类型必须都为input类型  
var schema = buildSchema(`
  input MessageInput {
    content: String
    author: String
  }

  type Message {
    id: ID!
    content: String
    author: String
  }

  type Query {
    getMessage(id: ID!): Message
  }

  type Mutation {
    createMessage(input: MessageInput): Message
  }
`);

class Message {
  constructor(id, {content, author}) {
    this.id = id;
    this.content = content;
    this.author = author;
  }
}

var root = {
  getMessage: ({id}) => {
    ...
    return new Message(...);
  },
  createMessage: ({input}) => {
    ...
    return new Message(...);
  }
};
```    

## GraphQL生态圈
Clients: Apollo, Relay, graphiql       

## GraphQL Nodejs实践坑
1. 每个Schema至少得有一个Query的Field         
2. syntax-error-unterminated-string，这是碰到块级字符串时容易出现的问题，有两种方案：一种是按GraphQL规范，使用"""封装块级字符串，另一种方案是使用GraphQL变量来接收块级字符串           


## 参考          
[GraphQL](https://graphql.org/graphql-js/)    
[GraphQL Spec](https://graphql.github.io/graphql-spec/June2018/#sec-String-Value)         
[syntax-error-unterminated-string](
https://stackoverflow.com/questions/58042147/strapi-graphql-mutation-syntax-error-unterminated-string)    
[GraphQL生态圈](https://zhuanlan.zhihu.com/p/30701842)    