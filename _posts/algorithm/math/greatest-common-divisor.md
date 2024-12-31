## 最大公因数（Greatest Common divisor）

能够整除多个非零整数的最大正整数

### 欧基里德算法

> a % b == 0，即 a 和 b 的最大公约数是 b；

> a 和 b 的最大公约数，等于 b 和 a % b 的最大公约数；

```JavaScript
function gcd(a, b) {
  if (a == 0) return b;
  if (b == 0) return a;
  let x1 = a;
  let x2 = b;
  while(x2 != 0) {
    const temp = x1;
    x1 = x2;
    x2 = temp % x2;
  }
  return x1;
}
```

[你或许希望理解数学证明，或者在这个问题上更进一步（点击打开链接）](https://oi-wiki.org/math/number-theory/gcd/#%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E7%AE%97%E6%B3%95)

## 附：最小公倍数（Least Common multiple）

> 多个数的倍数称为公倍数，除 0 外最小的公倍数称为最小公倍数

> 一般实现上，会令 0 和任意数的最小公倍数 = 0，但这实际上并不符合最小公倍数的定义。

两数相乘 = 最大公因数 x 最小公倍数

```md
证明如下：

（1）有最大公因数 gcd_m = GCD(a,b)

存在 m1 _ gcd_m = a, m2 _ gcd_m = b;

（2）有最小公倍数 lcm_n = LCM(a,b)

则存在 lcm_n / n1 = a , lcm_n / n2 = b;

（3）根据最大公因数与最小公倍数定义，m1 和 m2 互质，n1 和 n2 互质

（4）由（1）（2）可得

b/a = m2/m1 = n1/n2

即存在常数 y, m2/m1 = (y _ n1) / (y _ n2)

转换后 m2 = (y _ n1), m1 = (y _ n2)

（5）因为 m1、m2 互质，仅存在唯一公因数 1，即 y = 1

所以有 m2 = n1, m1 = n2

综上可得：a _ b = gcd_m _ lcm_n
```

```JavaScript
function lcm(a, b) {
  if (a === 0 || b === 0) return 0;
  return a * b / gcd(a, b);
}
```
