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

### Strings and UTF-8 
AMF 0使用（未修改的）UTF-8对字符串进行编码。UTF-8是8位Unicode转换格式的缩写。UTF-8字符串前面通常有一个字节长度的头，后面是一系列可变长度（1到4个八位字节）编码的Unicode码点。根据格式和数据类型，AMF可以使用稍微修改的ByTeleLength报头。下文将明确定义这些变体，并在整个文件中提及。

在ABNF语法中，[RFC3629]对UTF-8的描述如下：
- UTF8-char = UTF8-1 | UTF8-2 | UTF8-3 | UTF8-4
- UTF8-1 = %x00-7F
- UTF8-2 = %xC2-DF UTF8-tail
- UTF8-3 = %xE0 %xA0-BF UTF8-tail | %xE1-EC 2(UTF8-tail) | %xED %x80-9F UTF8-tail | %xEE-EF 2(UTF8-tail)
- UTF8-4 = %xF0 %x90-BF 2(UTF8-tail) | %xF1-F3 3(UTF8-tail) | %xF4 %x80-8F 2(UTF8-tail)
- UTF8-tail = %x80-BF 
