# AMF 0 数据类型
## 类型概述
AMF 0中有16个核心类型标记。类型标记的长度为一个字节，用于描述随后可能出现的编码数据类型。
```
marker = U8
```
下面列出了一组可能的类型标记（值以十六进制格式表示）：
- number-marker = 0x00
- boolean-marker = 0x01
- string-marker = 0x02
- object-marker = 0x03
- movieclip-marker = 0x04 ; reserved, not supported
- null-marker = 0x05
- undefined-marker = 0x06
- reference-marker = 0x07
- ecma-array-marker = 0x08
- object-end-marker = 0x09
- strict-array-marker = 0x0A
- date-marker = 0x0B
- long-string-marker = 0x0C
- unsupported-marker = 0x0D
- recordset-marker = 0x0E ; reserved, not supported
- xml-document-marker = 0x0F
- typed-object-marker = 0x10

注意：在Flash Player 9中引入AMF 3后，AMF 0中添加了一个特殊的类型标记，以发出切换到AMF 3序列化的信号。这允许NetConnection请求在AMF 0中启动，并在第一个复杂类型上切换到AMF 3，以利用AMF 3更高效的编码。

- avmplus-object-marker = 0x11 

类型标记后面可能跟有实际编码的类型数据，或者如果标记表示一个可能的值（例如null），则无需编码进一步的信息。
```
value-type = number-type | boolean-type | string-type |
             object-type | null-marker | undefined-marker |
             reference-type | ecma-array-type |
             strict-array-type | date-type | long-string-type
             | xml-document-type | typed-object-type 
```

对象结束类型(object-end-type) 应仅显示为标记 对象类型(object-type) 或 类型化对象类型(typed-object-type) 的一组属性的结束，或显示为ECMA数组关联部分的结束。

序列化不支持 Movieclip 和 Recordset 类型；它们的标记以保留状态保留，以备将来使用。

### 数字类型(Number Type)
AMF 0 数字类型用于编码ActionScript数字。数字类型标记后面的数据始终是网络字节顺序(network byte order)的8字节IEEE-754双精度浮点值（sign bit in low memory）。
```
number-type = number-marker DOUBLE 
```

### 布尔类型(Boolean Type)
AMF 0布尔类型用于对基本ActionScript 1.0或2.0布尔或ActionScript 3.0布尔进行编码。ActionScript 1.0或2.0布尔值的对象（非原始）版本不可序列化。布尔型标记后跟一个无符号字节；零字节值表示false，而非零字节值（通常为1）表示true。
```
boolean-type = boolean-marker U8 ; 0 is false, <> 0
                                 ; is true 
```

### 字符串类型(String Type)
AMF中的所有字符串都使用UTF-8编码；但是，字节长度标头格式可能会有所不同。AMF 0字符串类型使用标准字节长度头（即U16）。对于需要超过65535字节才能在UTF-8中编码的长字符串，应使用AMF 0长字符串类型。
```
string-type = string-marker UTF-8 
```

### 对象类型(Object Type)
AMF 0对象类型用于编码 匿名ActionScript对象(anonymous ActionScript objects)。任何没有注册类(registered class)的类型化对象都应被视为匿名ActionScript对象。如果同一对象实例出现在对象图中，则应使用AMF 0通过引用发送。

使用引用类型可以减少序列化的冗余信息和循环引用的无限循环。

```
object-property = (UTF-8 value-type) |
                  (UTF-8-empty object-end-marker)
anonymous-object-type = object-marker *(object-property) 
```
### Movieclip Type 
此类型不受支持，保留供将来使用。

### null 类型(null Type)
空类型由空类型标记表示。没有为该值编码更多信息。
```
null-type = null-marker 
```

### 引用类型(Reference Type)
AMF0 将 复杂对象(complex object) 定义为 匿名对象(anonymous object)、类型化对象(typed object)、数组(array) 或 ecma数组(ecma-array)。如果复杂对象的同一实例在对象图中出现多次，则必须通过引用发送。引用类型使用一个无符号16位整数来指向先前序列化对象表中的索引。指数从0开始。

```
reference-type = reference-marker U16 ; index pointing
                                       ; to another
                                       ; complex type 

```
16位无符号整数表示理论上最多可通过引用发送65535个唯一复杂对象。

### ECMA 数组类型(ECMA Array Type)
当ActionScript数组包含非顺序索引时，使用ECMA数组或“关联”数组。此类型被视为复杂类型，因此可以通过引用发送重复出现的实例。所有索引，序号或其他，都被视为字符串键，而不是整数。出于序列化的目的，此类型与匿名对象非常相似。
```
associative-count = U32
ecma-array-type = associative-count *(object-property)
```
32位关联计数表示理论上最大关联数组项数 4294967295。

### 对象结束类型(Object End Type)
对象结束标记用于一种特殊类型，表示匿名对象、类型化对象或关联数组中一组对象属性的结束。在这些类型之外不应出现这种情况。此标记前面总是有一个空UTF-8字符串，并一起构成对象结束类型。









