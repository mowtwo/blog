---
title: 前端中的计算机网络
date: 2022/02/21 14:00
description: 开始复习并整理一些计算机网络的知识
tag: 计算机网络
author: Mowtwo
---

# 前端中的计算机网络

## 三次握手与四次挥手

### 图例

![img](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/20170104214009596)

![img](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/d000baa1cd11728bf839be44c8fcc3cec3fd2cbb)

### 为什么需要三次握手与四次挥手

因为TCP是一种无状态的协议，所有的行为都是客户端发起请求，然后服务器进行响应的模式，因此在一次请求过程中，客户端随时都可能存在断开连接的情况。

因此在创建连接时，要多次确定连接的打开状态与实时性，而三次握手中第三次是最重要的，第三次结束后代表着连接成功，这时客户端可以跟服务端正常交换数据。

> 这里用人话解释一下：
>
> 当客户端发起第一次握手时，询问的就是请求创建连接，此时服务端允许创建，将允许返回后，客户端收到。但是要考虑到返回的允许报文是有可能丢失或者出现错误的，因此客户端并不一定会理解服务端的结果。此时客户端可能就已经停止了工作，然后假设此时服务端不需要第三次握手确认，就会让服务端进入空等待状态，一直等待客户端的操作，但是客户端此时已经不可能在连接进行任何操作，这样性能的浪费就产生了。
>
> 但是若有第三次确认，则可以确认客户端确实知道服务端允许创建连接了，那此后等待就都是有意义的。
>
> 因此在最理想状态下，三次握手是最合理的，那么是否可以大于三次呢，是可以的，只不过这样也会相应的增加创建连接的耗时。

四次挥手其实本质的作用三次握手是一致的，依然是为防止数据报文没有正确传达。

> 四次挥手中，其实在第三次挥手时，服务端就已经断开了连接，但是为了能保证服务端返回的关闭连接的报文能够正确传达，所以必须等到第四次客户端知道服务端关闭的回执。

### 更多的理解

上述的说法其实更多贴合的是教科书上讲法，以及为了好理解，实际上的tcp设计是很复杂的。

其中三次握手最重要的点可能就是防止同一个连接的多次请求，造成这种情况的原因其实是很常见的，那就是超时（timeout）。

> 这里要注意，超时的结果实际上是客户端决定的，但是超时的原因却有很多，有可能是网络阻塞，或者服务端处理太慢。当超时产生时，客户端就会放弃上一次请求的结果，产生一个全新的请求，此时请求的序列号偏移已经发生，而上一条请求里携带的是旧的偏移，这也是保证请求正确的关键。

网络请求实际上本身就是IO操作，而且是超距离传输的IO，IO本身就是耗时超过，而网络连接是一个很频繁的行为，倘若一个服务过渡的等待同步结果，则会让服务性能下降，虽然可以通过多线程之类的技术挂起等待，但是过多的挂起会让资源被浪费，因此耗时操作就会被设置一个最长等待时间。

但是网络请求过程中是需要确认（syn，同步）请求序列号来保证数据正确，而当新的请求发送出去时，若此时服务端接到了（或者早就接到，但是此时才返回）了上一次请求的第一次握手，然后返回了确定信息。客户端接收响应报文时检测了序列号，发现跟新的请求无法匹配，则会发发送一个`RST`报文来中止请求（此时会把自己新发的第二次握手也放弃，因为这样才能清空所有状态，让更新的请求能够完全同步，倘若不清空，第二次请求结果返回时，客户端会错误的应答两次，此时创建的连接可能就有两个）。

> [关于三次握手和四次挥手，面试官想听到怎样的回答？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/271701044)

![img](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/v2-ac0ecdad293a096d6b9402231af9f202_720w.jpg)

---



![img](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/v2-86544d47968e5c63ca62a222e7d6fa32_720w.jpg)

==这里不再完整举例挥手==

![img](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/v2-2085b747725cd09fb01334cd29774b6d_720w.jpg)

---

![img](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/v2-673c20366b9e4a9586538a93332692aa_720w.jpg)

## 如何在浏览器上发起网络请求

### 使用表单

表单是在传统互联网开发中常用的一种请求发起元素，一般对应HTML元素中的`form`

```html
<form action="/form.html" method="get">
    <input type="text" name="name">
    <br>
    <button type="submit">done</button>
</form>
```

### 使用XHR

在现代前后端分离的开发中，表单元素很少被直接使用到，因为表单会导致页面的刷新，这样前后端分离（或者说SPA）的本质目的所不同，所以我们经常会用到AJAX（Asynchronous Javascript And XML）技术。

不过由于每个浏览器上对于AJAX的实现是不同的（虽然目前而言，常见的浏览器中就IE不一样），所以这里举例的时候直接采用的Chrome中的`XMLHttpRequest`对象。

```javascript
const xhr = new XMLHttpRequest()
xhr.addEventListener('readystatechange', () => {
    console.log(xhr.readyState)
})
xhr.open('GET', '/xhr.html')
xhr.send(null)
```

### 使用fetch

这是一种浏览器提供的，全新的网络请求发送工具（或者说函数），基于Promise，而且默认禁用了Cookies的携带，更加安全，接口设计的更加简洁。但是由于轻量化带来了一些其他问题，比如在之前是无法主动撤销一次请求（实际上指的是放弃后续的返回报文，因为实际上服务端依然在处理请求）。

不过后续更新的AbortController解决了这个问题。

```javascript
fetch('/fetch.html', { method: 'GET' }).then(() => {
    console.log('result')
}).catch(reason => {
    console.log(reason)
})
```

> 可能还有一些其他的请求方式，不过并不需要一一列举，因为并不常有。此处暂时不列举websocket，因为这个比较特殊，而UDP也不是常用场景，也不讨论。

## 请求的过程观察

上述的三种请求方式虽然看上去形态各异，但是本质目的依然是为了发起HTTP请求，而且它们都会在浏览器上产生一个请求日志（拿Chrome举例），观察请求日志就可以看到HTTP请求的过程与最终报文转行的内容结果。

下面我们拿刚刚写的基于XHR的html的网络日志来看。

### 常规信息（General）

这部分信息主要指的是请求过程中，数据报文携带的通用信息，它们会在请求与响应的时候一直被传递与携带。

![image-20220221161155030](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20220221161155030.png)

#### Request URL

它是我们请求的资源目标地址，这里我们直接请求html文件本身，因为地址就是文件本身所在的完整地址。

#### Request Method

这里存储的是网络请求的方法（method），在HTTP请求中，设定了几种常用的方法来规定不同场景下的应用，虽然在行为上都是差不多的，但是若根据不同协议，它们可以附带一些特定的报文信息以及处理特定的请求行为。

- GET 一般用于直接获取资源
- POST 用于传输数据/文件到服务端
- PUT 替换已有的数据，用于更新操作
- DELETE 用于删除资源
- OPTIONS 这是一种特殊的methods，用于检测服务端的支持信息

> 上述的method不是全部的method，但是method本身终究也是一种约定俗成的应用模式，你完全可以在其中一种method下做另一个method的工作，但是这是==不推荐==的，更多的还是要看后端那边提供的约束文档

#### Status Code

状态码，这是HTTP请求过程中，表示响应状态的另一个重要信息，直接存在与报文中。目前是有一系列标准的状态，但是在实际开发过程中，应用也是根据后端开发者的心情去定的，因此下面只是说明它各状态码在标准下的位置，而这些状态码是有限的，在自定义情况下，状态可以被无限扩展。

| 状态码 | 作用                                   | 描述                                       |
| ------ | -------------------------------------- | ------------------------------------------ |
| 200    | 请求成功，并正常返回                   | 服务端返回对应资源                         |
| 201    | 请求成功，并产生了新的资源             |                                            |
| 202    | 请求成功，但操作未完成                 | 配合201状态码                              |
| 301    | 请求成功，但是需要重定向，并产生重定向 | 当请求的资源被永久到新的地址时产生         |
| 302    | 请求成功，但是被重定向                 | 临时的重定向                               |
| 304    | 请求到了缓存数据                       |                                            |
| 400    | 请求错误，返回的结果一般是不对的       |                                            |
| 401    | 请求被拒绝，缺少权限                   | 一般用于未认证错误                         |
| 403    | 请求被拒绝                             | 不是认证引起的拒绝                         |
| 404    | 请求失败，资源不存在                   | 经典的404                                  |
| 500    | 服务器错误                             | 基本上就是很严重的错误了，服务都挂了的那种 |
| 502    | 服务器错误                             | 有可能是认为引起的网关拒绝，防御性错误     |

---

> 剩余的信息并不在讨论范围内

### 请求头（Request Header）

这部分是用来表示客户端在给服务端发送请求时所携带的所有元信息

## 未完待续

