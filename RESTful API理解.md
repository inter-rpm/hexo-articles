---
title:  RESTful API理解
date: 2017-12-26 15:38:53
tags:
---

# RESTful API理解

## 1. restful api的定义

简单来说，RESTful API 是基于HTTP协议产生的一种相对简单的API设计方案，属于无状态传输。RESTful 的核心是 everything is a “resource”，所有的HTTP action，都应该是相应resource上可以被操作和处理的，而API 就是对资源的管理操作，而这个具体操作是由 HTTP action 指定的。RESTful API 是目前比较成熟的一套网络应用程序的API设计理论，RESTful API 的出现使前后端设备之间进行交互时可以按照这个设计更规范、更容易交流。

## 2. 理解RESTful

围绕资源展开讨论，从资源的定义、获取、表述、关联、状态变迁等角度，列举一些关键概念并加以解释

- 资源与URI
- 统一资源接口
- 资源的表述
- 资源的链接
- 状态的转移

### 2.1 资源与URI

REST全称是表述性状态转移，是指资源，只要有被引用到的必要，它就是一个资源。
要让一个资源可以被识别，需要有个唯一标识，在Web中这个唯一标识就是URI(Uniform Resource Identifier)。 URI既可以看成是资源的地址，也可以看成是资源的名称。
URI的设计应该遵循可寻址性原则，具有自描述性

### 2.2 统一资源接口

RESTful架构应该遵循统一接口原则，统一接口包含了一组受限的预定义的操作，不论什么样的资源，都是通过使用相同的接口进行资源的访问。接口应该使用标准的HTTP方法如GET，PUT和POST，并遵循这些方法的语义。

#### GET

- 安全且幂等
- 获取表示
- 变更时获取表示
- 200（ok）- 表示已在响应中发出
- 204（无内容）- 资源有空表示
- 301（Moved Permanently） - 资源的URI已被更新
- 303（See Other） - 其他（如，负载均衡）
- 304（not modified）- 资源未更改（缓存）
- 400 （bad request）- 指代坏请求（如，参数错误）
- 404 （not found）- 资源不存在
- 406 （not acceptable）- 服务端不支持所需表示
- 500 （internal server error）- 通用错误响应
- 503 （Service Unavailable）- 服务端当前无法处理请求

#### POST

- 不安全且幂等
- 使用服务端管理的（自动产生）的实例号创建资源
- 创建子资源
- 部分更新资源
- 如果没有被修改，则不过更新资源（乐观锁）
- 200（OK）- 如果现有资源已被更改
- 201（created）- 如果新资源被创建
- 202（accepted）- 已接受处理请求但尚未完成（异步处理）
- 301（Moved Permanently）- 资源的URI被更新
- 303（See Other）- 其他（如，负载均衡）
- 400（bad request）- 指代坏请求
- 404 （not found）- 资源不存在
- 406 （not acceptable）- 服务端不支持所需表示
- 409 （conflict）- 通用冲突
- 412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
- 415 （unsupported media type）- 接受到的表示不受支持
- 500 （internal server error）- 通用错误响应
- 503 （Service Unavailable）- 服务当前无法处理请求

#### PUT

- 不安全但幂等
- 用客户端管理的实例号创建一个资源
- 通过替换的方式更新资源
- 如果未被修改，则更新资源（乐观锁）
- 200 （OK）- 如果已存在资源被更改
- 201 （created）- 如果新资源被创建
- 301（Moved Permanently）- 资源的URI已更改
- 303 （See Other）- 其他（如，负载均衡）
- 400 （bad request）- 指代坏请求
- 404 （not found）- 资源不存在
- 406 （not acceptable）- 服务端不支持所需表示
- 409 （conflict）- 通用冲突
- 412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
- 415 （unsupported media type）- 接受到的表示不受支持
- 500 （internal server error）- 通用错误响应
- 503 （Service Unavailable）- 服务当前无法处理请求

#### DELETE

- 不安全但幂等
- 删除资源
- 200 （OK）- 资源已被删除
- 301 （Moved Permanently）- 资源的URI已更改
- 303 （See Other）- 其他，如负载均衡
- 400 （bad request）- 指代坏请求
- 404 （not found）- 资源不存在
- 409 （conflict）- 通用冲突
- 500 （internal server error）- 通用错误响应
- 503 （Service Unavailable）- 服务端当前无法处理请求

### 2.3 资源的表述

客户端获取的只是资源的表述而已。 资源在外界的具体呈现，可以有多种表述(或成为表现、表示)形式，在客户端和服务端之间传送的也是资源的表述，而不是资源本身
资源的表述包括数据和描述数据的元数据

### 2.4 资源的链接

超媒体的概念：把一个个把资源链接起来， 要达到这个目的，就要求在表述格式里边加入链接来引导客户端

### 2.5 状态的转移

无状态通信原则，并不是说客户端应用不能有状态，而是指服务端不应该保存客户端状态。

### 2.5.1 应用状态与资源状态

状态应该区分应用状态和资源状态，客户端负责维护应用状态，而服务端维护资源状态。 客户端与服务端的交互必须是无状态的，并在每一次请求中包含处理该请求所需的一切信息。 服务端不需要在请求间保留应用状态，只有在接受到实际请求的时候，服务端才会关注应用状态。 这种无状态通信原则，使得服务端和中介能够理解独立的请求和响应。 在多次请求中，同一客户端也不再需要依赖于同一服务器，方便实现高可扩展和高可用性的服务端

### 2.5.2 应用状态的转移

 “会话”状态不是作为资源状态保存在服务端的，而是被客户端作为应用状态进行跟踪的。客户端应用状态在服务端提供的超媒体的指引下发生变迁。服务端通过超媒体告诉客户端当前状态有哪些后续状态可以进入。
