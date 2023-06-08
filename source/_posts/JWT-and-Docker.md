---
title: JWT and Docker
date: 2023-06-03 23:31:59
tags:
    - JWT
    - Docker
    - Authentication
    - Security
    - Microservices
categories: Developer Guide
---
# JWT
SON Web Token（JWT）是一种用于在网络应用中进行身份验证和信息传递的开放标准（RFC 7519）。JWT由三部分组成：头部（Header）、载荷（Payload）和签名（Signature）。

## JWT的结构：

- 头部（Header）：包含关于令牌的元数据和签名算法信息，通常由两部分组成：令牌类型（如"JWT"）和所使用的算法（如HMAC SHA256或RSA）。
- 载荷（Payload）：包含要传输的声明（claims），声明是关于实体（通常是用户）和其他数据的声明性语句。声明可以是预定义的（比如：发行者、到期时间）或自定义的。
- 签名（Signature）：使用头部中指定的算法和密钥对头部、载荷进行签名，以确保令牌的完整性和真实性。

```javascript
// Encoded
// wernjdjbvvcjewrioopvcioh934fkbn9hvWK$Njbfvhuon34nnmvoinddklfgjlkjdlskfgkldfsnjvdf...乱码

// Decoded

// Header
{
    'alg': 'HS256',
    'typ': 'JWT'
}

// Payload
{
    'sub': '1235467890',
    'name': 'gzh',
    'iat': 1516239022
}

//Verify Signature
HMACSHA256{
    base64UrlEncode(header) + '.' + 
    base64UrlEncode(playload),
    服务器私钥
}
```


## JWT的工作流程：

- 用户进行身份验证后，服务器生成JWT并将其作为响应的一部分发送给客户端。
- 客户端将JWT存储（通常是在本地存储或Cookie中）以便后续的请求中使用。
- 客户端在后续的请求中将JWT作为授权凭证添加到请求的头部或参数中。
- 服务器在接收到请求时，通过验证JWT的签名和有效性来验证用户的身份。

注意：记得使用环境变量来隐藏私钥（gitignore）
```js
// 安装jsonwebtoken库：npm install jsonwebtoken

const jwt = require('jsonwebtoken');

// 创建JWT并生成令牌
const createToken = (user) => {
  // 设置Payload（声明）
  const payload = {
    id: user.id,
    username: user.username,
    // 可以添加其他自定义声明
  };

  // 生成令牌并返回
  const token = jwt.sign(payload, 'your-secret-key', { expiresIn: '1h' });
  return token;
};

// 验证和解码JWT令牌
const verifyToken = (token) => {
  try {
    const decoded = jwt.verify(token, 'your-secret-key');
    return decoded;
  } catch (error) {
    throw new Error('Invalid token');
  }
};

// 示例用法
const user = { id: 1, username: 'exampleuser' };

// 创建JWT
const token = createToken(user);
console.log('Token:', token);

// 验证和解码JWT
try {
  const decodedToken = verifyToken(token);
  console.log('Decoded Token:', decodedToken);
} catch (error) {
  console.error('Token verification failed:', error.message);
}
```

## 附加知识点
### JWT（JSON Web Token）为什么将令牌（Token）放在头部
JWT（JSON Web Token）将令牌（Token）放在头部而不是其他地方，是因为在HTTP请求的头部使用Bearer Token认证方案更为标准和安全。将JWT放在头部的好处包括：

- 标准化：将JWT放在头部的方式符合标准的HTTP协议规范。在请求头部使用Authorization字段，并指定Bearer作为认证方案，可以让开发人员和服务器端轻松地理解和解析JWT令牌。

- 安全性：将JWT放在头部可以提供更好的安全性。由于JWT是明文传输的，将其放在头部可以减少在传输过程中被截获的风险。同时，将JWT放在头部可以避免浏览器发送请求时自动携带Cookie的安全问题。

- 灵活性：将JWT放在头部可以与其他身份验证机制（如基本身份验证、OAuth等）配合使用，而不会产生冲突或混淆。头部提供了统一的位置来存储JWT令牌，使其易于识别和管理。

### 关于Cookie、Session和Token的区别和身份认证的方式如下：

- Cookie：Cookie是存储在客户端的小型文本文件，由服务器在HTTP响应中通过Set-Cookie头部发送给客户端，并在后续的请求中通过Cookie头部发送回服务器。Cookie通常用于会话管理和跟踪用户状态，存储在浏览器中，具有一定的安全性问题，如跨站点脚本攻击（XSS）和跨站点请求伪造（CSRF）。

- Session：Session是服务器端存储的关于用户会话的信息。当用户通过身份验证登录后，服务器会创建一个唯一的会话标识符（Session ID），并将其发送给客户端（通常通过Cookie）。客户端在后续的请求中将Session ID发送回服务器，服务器根据Session ID检索相关的会话数据。Session通常存储在服务器的内存或持久化存储中，并提供更高的安全性，但需要在服务器上维护会话状态。

- Token：Token是一种用于身份认证和授权的令牌。与Cookie和Session不同，Token是无状态的，服务器不需要在后端存储会话信息。Token通常是基于JWT的，包含了用户的身份信息和其他相关的数据，可以在客户端和服务器之间进行安全传输。客户端在每次请求中将Token作为请求头或参数发送给服务器，服务器通过验证Token的签名和有效性来进行身份认证和授权。


# Docker
Docker 是一个开源的容器化平台，它可以将应用程序和其依赖项打包为一个独立的容器，使其能够在不同的环境中以一致的方式运行。
```Dockerfile
# 使用基础镜像
FROM node:14

# 设置工作目录
WORKDIR /app

# 复制依赖文件
COPY package*.json ./

# 安装依赖
RUN npm install

# 复制应用程序代码
COPY . .

# 暴露应用程序端口
EXPOSE 3000

# 运行应用程序
CMD ["npm", "start"]
```