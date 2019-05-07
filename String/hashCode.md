```
/** The value is used for character storage. */
private final char value[];

/** Cache the hash code for the string */
private int hash; // Default to 0
```

value：是一个 private final 修饰的 char 数组，String 类是通过该数组来存在字符串的。

hash：是一个 private 修饰的 int 变量，用来存放 String 对象的 hashCode。


```$xslt
public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
```
还是很好理解的
计算公式为   
s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]

通过查阅资料得到这个因子31使用原因：

在存储数据计算hash地址的时候，我们希望尽量减少有同样的hash地址，所谓“冲突”。如果使用相同hash地址的数据过多，那么这些数据所组成的hash链就更长，从而降低了查询效率！所以在选择系数的时候要选择尽量长(31 = 11111[2])的系数并且让乘法尽量不要溢出(如果选择大于11111的数，很容易溢出)的系数，因为如果计算出来的hash地址越大，所谓的“冲突”就越少，查找起来效率也会提高。

31可以 由i*31== (i<<5)-1来表示，现在很多虚拟机里面都有做相关优化，使用31的原因可能是为了更好的分配hash地址，并且31只占用5bits！

在java乘法中如果数字相乘过大会导致溢出的问题，从而导致数据的丢失.

而31则是素数（质数）而且不是很长的数字，最终它被选择为相乘的系数的原因不过与此！
