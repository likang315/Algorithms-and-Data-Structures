### Easy

------

[TOC]

##### 01：计算根号2的值，保留小数点后10位

```java
public static double squareTwo() {
    final double frequency = 0.00000000001;
    double left = 1.4;
    double right = 1.5;
    while (right - left > frequency) {
        double mid = (right + left) / 2.0;
        if (mid * mid > 2)
            right = mid;
        else
            left = mid;
    }

    return Double.parseDouble(Double.toString(left).substring(0,12));
}
```

##### 02：1，2，3，4 组成无重复的三位数

- 固定位数

```java
static void solution() {
    int count = 0;
    // i 百位，j 十位，k 个位
    for (int i = 1; i <= 4; i++) {
        for (int j = 1; j <= 4; j++) {
            for (int k = 1; k <= 4; k++) {
                if (i != j && i != k && j != k) {
                    int number = i * 100 + j * 10 + k;
                    System.out.println(number);
                    count++;
                }
            }
        }
    }
}
```

