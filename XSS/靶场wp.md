## pikachu

2.![image-20250324170030343](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170030462.png)

3.![image-20250324170751364](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170751432.png)

4.![image-20250324170810460](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170810530.png)

5.![image-20250324170825565](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170825616.png)

6.先转到后台登入就能看到之前盲打消失的内容 ![image-20250324170857580](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170857686.png)

## DVWA

1.low

url 直接改

![image-20250324170917561](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170917625.png)

1.medium

![image-20250324170935120](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170935233.png)

1.high 白名单只允许四个语言用#注释后的部分不发到后端过滤

![image-20250324170952165](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324170952265.png)

2.low

![image-20250324171009817](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324171009941.png)

2.medium 只过滤 

![image-20250324171033199](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324171033262.png)

2.high 过滤了 <script> 和其大小写用别的语句

![image-20250324171054064](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324171054130.png)

3.low

![image-20250324171110419](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324171110530.png)

3.medium   Name 部分只过滤 <script> 所以前端改一下 name 长度限制然后用其他语句或者大小写，双写就可以绕过

![image-20250324171133248](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324171133304.png)

3.high 和 medium 差不多只是大小写也不能用了

![image-20250324171326905](http://cdn.jsdelivr.net/gh/LynnJY/NoteDemo/20250324171327004.png)

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# @Time : 2025/3/24 16:12
# @Author : Lynn
# @Site : 
# @Describe:
import numpy as np

def strassen_multiply(A, B):
    """ Strassen 矩阵乘法，适用于 2^k x 2^k 的方阵 """
    n = A.shape[0]

    # 当矩阵足够小（如 1x1），直接相乘
    if n == 1:
        return A * B

    # 将 A 和 B 拆分为 4 个子矩阵
    mid = n // 2
    A11, A12, A21, A22 = A[:mid, :mid], A[:mid, mid:], A[mid:, :mid], A[mid:, mid:]
    B11, B12, B21, B22 = B[:mid, :mid], B[:mid, mid:], B[mid:, :mid], B[mid:, mid:]

    # 计算 7 个 Strassen 乘积
    M1 = strassen_multiply(A11 + A22, B11 + B22)
    M2 = strassen_multiply(A21 + A22, B11)
    M3 = strassen_multiply(A11, B12 - B22)
    M4 = strassen_multiply(A22, B21 - B11)
    M5 = strassen_multiply(A11 + A12, B22)
    M6 = strassen_multiply(A21 - A11, B11 + B12)
    M7 = strassen_multiply(A12 - A22, B21 + B22)

    # 计算 C 的 4 个分块
    C11 = M1 + M4 - M5 + M7
    C12 = M3 + M5
    C21 = M2 + M4
    C22 = M1 - M2 + M3 + M6

    # 组合 C 矩阵
    C = np.zeros((n, n), dtype=int)
    C[:mid, :mid] = C11
    C[:mid, mid:] = C12
    C[mid:, :mid] = C21
    C[mid:, mid:] = C22

    return C

def next_power_of_2(n):
    """ 计算大于等于 n 的最小 2^k """
    power = 1
    while power < n:
        power *= 2
        return power

    def pad_matrix(A):
        """ 若矩阵维度不是 2^k，则填充 0 使其变为 2^k x 2^k """
        n, m = A.shape
        max_dim = next_power_of_2(max(n, m))
        padded_A = np.zeros((max_dim, max_dim), dtype=int)
        padded_A[:n, :m] = A
        return padded_A

    # 生成两个 8x8 的随机整数矩阵
    size = 8  # 2^k
    A = np.random.randint(1, 10, (size, size))
    B = np.random.randint(1, 10, (size, size))

    # 确保是 2^k 维度
    A_padded = pad_matrix(A)
    B_padded = pad_matrix(B)

    # 执行 Strassen 矩阵乘法
    C_strassen = strassen_multiply(A_padded, B_padded)

    # 验证结果是否正确（与 NumPy 的矩阵乘法进行比较）
    C_numpy = np.dot(A_padded, B_padded)

    # 输出计算结果
    print("矩阵 A：\n", A)
    print("矩阵 B：\n", B)
    print("Strassen 乘法结果：\n", C_strassen[:size, :size])  # 还原原始大小
    print("NumPy 乘法结果：\n", C_numpy[:size, :size])

    # 验证结果是否一致
    if np.array_equal(C_strassen, C_numpy):
        print("✅ 结果正确！Strassen 乘法与标准矩阵乘法结果一致。")
    else:
        print("❌ 结果错误！请检查实现。")```echarts
        // Built-in variables:
            //   1. myChart: ECharts instance
            //   2. echarts: ECharts module
            //   3. option:  ECharts option object
            //   4. this:    ECharts plugin instance
            // More examples: https://echarts.apache.org/examples/en/index.html#chart-type-line
            // All code within this block will be evaluated (eval). Exercise caution.
            // Use the following comment to set chart dimensions (otherwise defaults apply):
                // {height: "300px", width: ""}

                option = {
                    tooltip: { trigger: 'item' },
                    legend: { top: '5%', left: 'center' },
                    series: [{
                        name: 'Access From',
                        type: 'pie',
                        radius: ['40%', '70%'],
                        avoidLabelOverlap: false,
                        label: { show: false, position: 'center' },
                        emphasis: { label: { show: true,  fontSize: 40,  fontWeight: 'bold' } },
                        labelLine: { show: false },
                        data: [
                            {value: 1548, name: 'Search Engine'},
                            {value: 735, name: 'Direct'},
                            {value: 580, name: 'Email'},
                            {value: 484, name: 'Union Ads'},
                            {value: 310, name: 'Video Ads'}
                        ]
                    }]
                }
```

```

