# 介绍

## 目的
动作消息格式(AMF, Action Message Format)是一种紧凑的二进制格式，用于序列化ActionScript对象图。一旦序列化，AMF编码的对象图(object graph)可以用于对跨越会话(session)的应用程序状态持久化以及检索,或者允许两个端点通过比较复杂的数据类型进行通信。AMF的第一个版本称为AMF 0，它序列化ActionScript对象并保留复杂类型信息，捕获应用程序数据的公共状态。AMF 0还支持通过引用发送复杂对象，这有助于避免在对象图中发送冗余实例，并允许端点恢复关系和避免循环引用。

## 符号约定(Notational Conventions)
### Augmented BNF
本规范中的类型定义使用扩展的Backus Naur Form（ABNF）语法[RFC2234]。在阅读本文件之前，读者应熟悉此符号。

## 基本规则
- U8 = An unsigned byte, 8-bits of data, an octet 
- U16 = An unsigned 16-bit integer in big endian (network) byte order
- S16 = An signed 16-bit integer in big endian (network)  byte order
- U32 = An unsigned 32-bit integer in big endian (network) byte order
- DOUBLE = 8 byte IEEE-754 double precision floating point value in network byte order (sign bit in low memory).
- KB = A kilobyte or 1024 bytes.
- GB = A Gigabyte or 1,073,741,824 bytes. 
