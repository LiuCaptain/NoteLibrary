# 动态字节数组与动态长度字符串

### 两者的关系和区别

1. #### 底层本质：`string` 在底层其实就是一个 **动态字节数组 (`bytes`)**，只不过**编译器和语义上限定它是 UTF-8 编码的文本**，而 `bytes` 可以存任意二进制数据。

   - ##### `string`：UTF-8 编码的动态字节序列

     ```solidity
     contract StringExample {
     	string public text = "你好"; // UTF-8 编码
     }
     ```

     **UTF-8 编码细节：**

     - `"你"` 的 UTF-8 编码是 `0xE4 0xBD 0xA0`（3 字节）
     - `"好"` 的 UTF-8 编码是 `0xE5 0xA5 0xBD`（3 字节）
     - 所以 `text` 实际存储的是 **[长度=6字节] [数据=E4 BD A0 E5 A5 BD]**

     **限制：**

     - Solidity 要求 `string` **必须是合法的 UTF-8 序列**，否则无法安全解析为字符串。

        > 只有在以下情况才可能出现“不合法 UTF-8”：
        >
        > 1. **直接用 `string(bytes)` 强制转换**：
        >
        >    ```solidity
        >    bytes memory b = hex"FF";
        >    string memory s = string(b); // ❌ 得到不合法的 UTF-8
        >    ```
        >
        >    这里 `0xFF` 就不是合法 UTF-8。
        >
        > 2. **外部调用传入不合规数据**：
        >    如果有人恶意构造 `calldata`，把不合法的字节数组作为 `string` 参数传进来，ABI 解码时可能出错。
        >
        > 3. **存储/拼接破坏了 UTF-8 边界**：
        >    如果手动拼接 `string` 时，没有保证字符边界完整，也可能生成不合法序列。
        >
        >    
        >
        > 对于我们写 `"中"` 这样的字面量，编译器会自动转成合法 UTF-8 字节序列。

     - 无法直接用下标改字符，因为一个字符可能由多个字节组成：

        ```solidity
        string s = "abc";
        s[0] = 0x64; // ❌ 不允许这样，编译错误
        ```

     - 读取或修改时，需要先转换成 `bytes`：

        ```solidity
        function changeFirstLetter() public {
          bytes memory b = bytes(text);
          b[0] = 0x64; // 改第一个字节（破坏UTF-8可能导致乱码）
          text = string(b);
        }
        ```

   - ##### `bytes`：任意二进制数据（动态长度）

     ```solidity
     contract BytesExample {
     	bytes public data = hex"FF00AA"; // 任意二进制数据
     }
     ```

     **特点：**

     - 可以存储任意字节，不必是 UTF-8，有效值范围 0x00 ~ 0xFF。
     
     - 可以直接按下标访问和修改：
     
       ```solidity
       function updateByte() public {
           data[0] = 0x11; // ✅ 直接修改
       }
       ```
     
     - 常用来存储加密签名、哈希值、图片二进制数据等。

2. #### 访问与修改

   - ##### `string` 的修改

     ```solidity
     contract StringDemo {
       function testString() public pure returns (string memory, bytes1, bytes1) {
         string memory s = "abc"; // 底层存储 [0x61, 0x62, 0x63]
     
         // ❌ 不能直接修改，编译报错
         s[0] = 0x64;
     
         // ✅ 必须先转成 bytes
         bytes memory b = bytes(s);
         b[0] = 0x64;  // 把第一个字节改为 0x64 ('d')
         string memory result = string(b);  
     
         return (result, b[0], b[1]);
       }
     }
     ```

     执行 `testString()` 的结果：

     - 修改后的字符串result：`"dbc"`
     - `b[0]`：`0x64` (即 `'d'`)
     - `b[1]`：`0x62` (即 `'b'`)

   - ##### `string` 的访问

     ```solidity
     contract StringAccess {
       function getChar() public pure returns (bytes1, bytes1) {
         string memory s = "abc";
         bytes memory b = bytes(s);  
     
         // 通过索引访问
         return (b[0], b[2]);
       }
     }
     ```

      执行 `getChar()` 的结果：

     - `b[0]` = `0x61` → `'a'`
     - `b[2]` = `0x63` → `'c'`

   - ##### `bytes` 的访问和修改

     ```solidity
     contract BytesDemo {
       function testBytes() public pure returns (bytes memory, bytes1, bytes1) {
         bytes memory b = "abc"; // 初始内容 [0x61, 0x62, 0x63] -> abc
     
         b[0] = 0x64; // 修改第一个字节 (0x61 -> 0x64)，现在是 [0x64, 0x62, 0x63] -> "dbc"
     
         return (b, b[0], b[1]);
       }
     }
     ```

     执行 `testBytes()` 的结果：

     - `b`：`"dbc"`
     - `b[0]`：`0x64` (即 `'d'`)
     - `b[1]`：`0x62` (即 `'b'`)

   **总结：**

   1. `bytes`：可以直接通过索引访问和修改。
   2. `string`：`string` 的修改需要先转换成 `bytes` 再修改，然后再重新转换成 `string`；`string` 的访问也需要先转换成 `bytes` 再访问。

3. 长度变化：两者在存储上都是动态长度（长度信息 + 数据本体），所以大小不是固定的

   `bytes` 和 `string` 在存储结构上是完全一样的，唯一的区别是：

   - `string` **语义上要求 UTF-8 文本**
   - `bytes` **语义上是任意字节序列**
   
   **EVM 根本不关心 UTF-8**，只认 **字节数组**
   
   
   
   
