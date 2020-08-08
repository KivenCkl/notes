# Go Notes

## 字符串高效拼接

1. `+` 号拼接

   ```go
   func StringPlus() string {
       var s string
       s += "hello" + " " + "world" + "!" + "\n"
       return s
   }
   ```

   这种拼接最简单。

2. `fmt` 拼接

   ```go
   func StringFmt() string {
       return fmt.Sprint("hello", " ", "world", "!", "\n")
   }
   ```

   借助 `fmt.Sprint` 系列函数进行拼接，性能远没有 `+` 号操作快。

3. `Join` 拼接

   ```go
   func StringJoin() string {
       s := []string{"hello", " ", "world", "!", "\n"}
       return strings.Join(s, "")
   }
   ```

   整体和 `+` 操作相差不了太多。

4. `buffer` 拼接

   ```go
   func StringBuffer() string {
       var b bytes.Buffer
       b.WriteString("hello")
       b.WriteString(" ")
       b.WriteString("world")
       b.WriteString("!")
       b.WriteString("\n")
       return b.String()
   }
   ```

   性能最好。

5. `builder` 拼接

   ```go
   func StringBuilder() string {
       var b strings.Builder
       var b bytes.Buffer
       b.WriteString("hello")
       b.WriteString(" ")
       b.WriteString("world")
       b.WriteString("!")
       b.WriteString("\n")
       return b.String()
   }
   ```

   `Buffer` 和 `Builder` 性能相差无几，`Builder` 在内存的使用上要略优于 `Buffer`。

