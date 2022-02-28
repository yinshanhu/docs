## 常用方法

### 生成指定长度随机字符

```c#
public string RandomString(int Length)
{
    string chars = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
    Random randrom = new Random((int)DateTime.Now.Ticks);
    string str = "";
    for (int i = 0; i < Length; i++)
    {
        str += chars[randrom.Next(chars.Length)];
    }
    return str;
}
```