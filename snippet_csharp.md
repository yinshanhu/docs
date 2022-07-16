## 语法

### 扩展方法

```cs

// a.cs
using Microsoft.EntityFrameworkCore; // ModelBuilder 的命名空间

namespace xxx {
    public class a: DbContext {
        protected override void OnModelCreating(ModelBuilder modelBuilder){
            // 此处想给 ModelBuilder 类扩展一个 Seed 方法。

            modelBuilder.Seed(); // 调用自定义扩展的 Seed 方法。
        }
    }
}


// ModelBuilderExtentions.cs 一般以类名 + Extentions 的方式命名
using Microsoft.EntityFrameworkCore; // ModelBuilder 的命名空间

namespace xxx {

    public static class ModelBuilderExtentions { // 关键点1：此类需要使用静态类，static

        public static void Seed(this ModelBuilder modelBuilder) { // 关键点2：在ModelBuilder前加this
            // 在此，处理需要处理的逻辑
            // modelBuilder.xxxx ...
        }
    }
}

```

## 常用方法

### 生成指定长度随机字符

```cs
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