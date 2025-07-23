

# C++题目

## GESP

### 一级

#### 1.1 多少分钟

小明在为自己规划学习时间。现在他想知道两个时刻之间有多少分钟，你能通过编程帮他做到吗？

**输入格式**

输入 4 行，第一行为开始时刻的小时，第二行为开始时刻的分钟，第三行为结束时刻的小时，第四行为结束时刻的分钟。输入保证两个时刻是同一天，开始时刻一定在结束时刻之前。时刻使用 24 小时制，即小时在 0 到 23 之间，分钟在 0 到 59 之间。

**输出格式**

输出一行，包含一个整数，从开始时刻到结束时刻之间有多少分钟。

**输入** 

9

5

9

6

**输出**

1

**输入**

9

5

10

0

**输出**

55

```cpp
#include <iostream>
using namespace std;

int main()
{
    // 定义开始时间和结束时间的小时和分钟
    int start_hour, start_minute, end_hour, end_minute;
    // 从标准输入读取开始时间和结束时间的小时和分钟
    cin >> start_hour >> start_minute >> end_hour >> end_minute;
    // 将开始时间和结束时间转换为分钟数
    int start_time = start_hour * 60 + start_minute;
    int end_time = end_hour * 60 + end_minute;
    // 输出结束时间与开始时间的分钟差
    cout << end_time - start_time << endl;
}
```

#### 1.2 这一天的第几秒

小明刚刚学习了小时、分和秒的换算关系。他想知道一个给定的时刻是这一天的第几秒，你能编写一个程序帮帮他吗？

**输入格式**

输入一行，包含三个整数和一个字符。三个整数分别表示时刻的时、分、秒；字符有两种取值，大写字母'A'表示上午，大写字母'P'表示下午。

**输出格式**

输出一行，包含一个整数，表示输入时刻是当天的第几秒。

**输入输出样例**

**输入**

0 0 0 A

**输出** 

0

**输入**

11 59 59 P

**输出**

86399

```cpp
#include <iostream>
using namespace std;

int main() 
{
    // 定义小时、分钟、秒和上午/下午的变量
    int h, m, s;
    char ap;
    // 从标准输入读取小时、分钟、秒和上午/下午
    cin >> h >> m >> s >> ap;
    // 将小时、分钟、秒转换为总秒数
    int total_seconds = h * 3600 + m * 60 + s;
    // 如果是下午且小时不是12，则将总秒数加上12小时的秒数
    if (ap == 'P' && h != 12)
    {
        total_seconds += 12 * 3600;
    // 如果是上午且小时是12，则将总秒数减去12小时的秒数  24:00 00:00
    } else if (ap == 'A' && h == 12) 
    {
        total_seconds -= 12 * 3600;
    }
    // 输出总秒数
    cout << total_seconds << endl;
}
```

#### 1.3 青蛙公主

这是一口很深的井（相对于青蛙而言），井壁由m块圆环形的砖头构成，每块砖头的高度都是p，青蛙每天白天最多能够向上爬的高度是q。

但是，很不幸的是，晚上砖头会渗水，会变得很滑，如果青蛙不能藏在两块砖头之间，那么青蛙将会滑落回井底。

还好青蛙是公主变的，还保留了公主的智商，在确定当天无法爬出井口的情况下，会选择白天结束前躲在尽可能高的砖缝之间，避免晚上跌落。

问：青蛙要花几天才能爬出来。

**输入描述**

一个正整数n，表示案例的数量。

每组案例由三个正整数m、p、q组成。(m<=5000, p<=1000, q<=1000)

**输出描述**

针对每组案例，输出一个整数，表示爬出来需要的天数。如果无法爬出来，则输出-1。

**样例输入**

2

5 2 3

5 3 2

**样例输出**

5

-1

```cpp
#include <iostream>

int main() {
    int n; // 案例数量
    std::cin >> n;

    while (n--) {
        long long m, p, q; // 使用 long long 防止 m*p 溢出
        std::cin >> m >> p >> q;

        long long total_depth = m * p; // 井的总深度
        long long current_height = 0;  // 青蛙当前高度
        int days = 0;                  // 天数

        // 特殊情况：如果青蛙每天爬的高度 q 小于一块砖的高度 p，
        if (q < p )
         { // m > 0 确保井不是0深度
            std::cout << -1 << std::endl;
            continue; // 处理下一个案例
        }
        
        while (true)
        {
            days++; // 天数增加

            // 白天爬升
            current_height += q;

            // 判断是否出井
            if (current_height >= total_depth) {
                std::cout << days << std::endl;
                break; // 青蛙已爬出
            }

            // 晚上处理：如果未出井，躲到尽可能高的砖缝
            // 由于青蛙会滑回井底的条件是“不能藏在两块砖头之间”，
            // 且它会“选择白天结束前躲在尽可能高的砖缝之间”，
            // 这意味着青蛙能安全过夜的高度必须是 p 的整数倍。
            // 并且，如果它爬不到第一块砖头的高度 p，它就会掉回井底。
            // 所以，如果 current_height < p，它当晚就会回到0。
            // 如果 current_height >= p，它会停留在 (current_height / p) * p 的位置。
            if (current_height < p) 
            {
                 // 如果白天爬了 q，但是连一块砖的高度都够不上，
                 // 且之前不是在井口，那么它当天晚上会滑回井底。
                 // 这种情况意味着永远无法爬出。
                 // 这个条件已经在循环前 if (q < p && m > 0) 覆盖了
                 // 理论上如果 q >= p，则 current_height 至少会是 p
                 // 所以这里可以不用特别处理 current_height = 0 的情况
                 std::cout << -1 << std::endl; // 此时意味着无法爬上第一块砖，永远出不来
                 break;
            } else 
            {
                current_height = (current_height / p) * p; // 青蛙停留在能过夜的最高砖缝
            }
             
            // 检查是否陷入无限循环（例如，每次爬升后，晚上都会滑回同样的高度，无法进步）
            // 如果 current_height 在某次迭代结束后没有增长，并且还没出井，则陷入死循环
            // 考虑到如果 q < p，已经在前面处理了 -1 的情况
            // 如果 q >= p，那么每次 current_height 至少能增加 q - (q % p)
            // 每次循环至少有 progress >= p
            // 实际上，如果青蛙能爬过一块砖的高度，它就总能进步
            // 所以陷入死循环的唯一情况是 q < p，这已经在前面处理。
            // 这里不需要额外的死循环判断
        }
    }

    return 0;
}
```



#### 1.4 日字矩阵

小杨想要构造一个 N×N 的日字矩阵（N 为奇数），具体来说，这个矩阵共有 N 行，每行 N 个字符，其中最左列、最右列都是 |，而第一行、最后一行、以及中间一行（即第(N+1)/2行）的第 2~N-1 个字符都是 - ，其余所有字符都是半角小写字母 x 。例如，一个 N=5 日字矩阵如下:
	|- - -|
	|xxx|
	|- - -|
	|xxx|
	|- - -|

请你帮小杨根据给定的 N 打印出对应的“日字矩阵”

**输入格式**
        一个整数 N（5≤N≤49，保证 N 为奇数）。

**输出格式**
        输出对应的“日字矩阵”。

**输入输出样例
        输入**
	5
	**输出**
	|- - -|
	|xxx|
	|- - -|
	|xxx|
	|- - -|
	**输入**
	7
	**输出**
	|- - - - -|
	|xxxxx|
	|xxxxx|
	|- - - - -|
	|xxxxx|
	|xxxxx|
	|- - - - -|

```cpp
#include <iostream>
using namespace std;

int main()
{
	// 定义一个整数n
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			// 如果j等于1或者n，则输出"|"
			if (j == 1 || j == n)
			{
				cout << "|";
			}
			// 如果i等于1或者n，则输出"-"
			else if( i==1|| i==n || i == (n + 1) / 2)
			{
				cout<<"-";
			}
			// 否则输出"x"
			else
			{
				cout << "x";
			}
		}
		// 换行
		cout << endl;
	}
	return 0;
}
```

#### 1.5 第二件半价

超市促销，第二件半价，之后的每一件都是上一件的半价。

假设某商品的原价是 a 元，现在需要购买 b 件该商品，输出需要花多少钱。

**输入描述**

两个正整数 a 和 b 含义如描述。（1 ≤ a、b ≤ 10）

**输出描述**

在一行中输出需要花的钱数，保留两位小数。

**样例输入**

8 4

**样例输出**

15.00

```cpp
#include <iostream>

// 使用命名空间，方便书写
using namespace std;

int main() 
{
    // 优化输入输出，提高效率
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int a, b; // a: 原价, b: 购买数量
    cin >> a >> b;

    double total_cost = 0.0;
    double current_price = a; // 第一件商品的原价

    // 第一件商品
    total_cost += current_price;

    // 从第二件商品开始计算
    for (int i = 2; i <= b; ++i) 
    {
        current_price /= 2.0; // 每一件是上一件的半价
        total_cost += current_price;
    }

    // 设置输出精度为两位小数
    printf("%.2f\n", total_cost);

    return 0;
}
```

#### 1.6 坤兔同笼

鸡和兔关在一个笼子里，鸡有2只脚，兔有4只 脚。已知现在可以看到笼子里有m个头和n只脚，求鸡和兔子 各有多少只？分别输入两个数分别为头的数目和脚的数目，求出鸡的数量和兔的数量，如果有解，输出鸡的数量和兔的数量，如果无解，则输出Error。

 *算法分析：*

 *1、设鸡有x只，兔有y只，则：x+y=m ，2x+4y=n ，将两个方程代入求解，x=2*m-n/2，y=n/2-m;

 *2、判断脚的数量是否为偶数，如果不是偶数，则出错;*

 3、如果是偶数，则判断一下x和y是不是都大于等于0，如果是，说明有解，否则出 错。

 **输入描述**

 输入两个正整数m和n

 **输出描述**

 依次输出鸡和兔的数量，空格隔开。无解时输 出”Error”。 

 **样例输入**

 2 6

 **样例输出**

 1 1

```cpp
#include <iostream>
using namespace std;

int main()
{
    int m, n;
    cin >> m >> n;

    // 脚必须是偶数
    if (n % 2 != 0)
    {
        cout << "Error" << endl;
        return 0;
    }

    int x = 2 * m - n / 2; // 鸡的数量
    int y = n / 2 - m;     // 兔的数量

    if (x >= 0 && y >= 0)
    {
        cout << x << " " << y << endl;
    }
    else
    {
        cout << "Error" << endl;
    }
}

```

### 二级

#### 2.1 找最小Y

有一个整数 x，找到最小的正整数 y 使得下式成立：

*(x and y)+(x or y)=2025*

其中 and 表示二进制按位与（&）运算，or 表示二进制按位或（|）运算。如果不存在满足条件的 y，则输出no。 | 

**输入格式**

一行，一个整数 x。

**输出格式**

一行，一个整数，若满足条件的 y 存在则输出 y，否则输出 no。

**输入输出样例**

**输入**

1025

**输出**

1000



**解析：**

题目化成(x & y) + (x | y) = x + y

因为每一位的按位与 & 和按位或 | 相加，等价于两个数的普通加法。

原理：

x = 6  = 0110

y = 3  = 0011

x & y = 0010 = 2

x | y = 0111 = 7

x + y = 0b0111 = 7

所以 (x & y) + (x | y) = x + y

```cpp
#include <iostream>
using namespace std;
//解法1 (x & y) + (x | y) = x + y
void fun01(int x)
{
    //定义变量y，表示2025减去x的值
    int y = 2025 - x;

    //如果y大于0，则输出y
    if (y > 0)
        cout << y << endl;
    //否则输出-1
    else
        cout << -1 << endl;
}
//解法2，枚举y，判断(x & y) + (x | y)是否等于2025
// 函数fun02，参数为整数x
void fun02(int x)
{
    // 循环变量y从1到2025
    for (int y = 1; y <= 2025; y++)
        // 如果x与y按位与的结果加上x与y按位或的结果等于2025
        if ((x & y) + (x | y) == 2025)
        {
            // 输出y
            cout << y << endl;
            // 返回
            return;
        }
    // 如果没有找到符合条件的y，输出-1
    cout << "-1" << endl;
}
int main()
{
    // 定义一个整数变量x
    int x;
    // 从标准输入流中读取一个整数，并将其赋值给变量x
    cin >> x;
    // 输出第一个方法
    cout << "第一个方法：" << endl;
    // 调用fun01函数，并将x作为参数传入
    fun01(x);
    // 输出第二个方法
    cout << "第二个方法：" << endl;
    // 调用fun02函数，并将x作为参数传入
    fun02(x);
}

```

#### 2.2 勾股数

勾股数是很有趣的数学概念。如果三个正整数 a,b,c，满足 a^2^ +b^2^ =c^2^，而且 1≤a≤b≤c，我们就将 a,b,c 组成的三元组 (a,b,c) 称为勾股数。你能通过编程，数数有多少组勾股数，能够满足 c≤n 吗？

**输入格式**

输入一行，包含一个正整数 n。约定 1≤n≤1000。

**输出格式**

输出一行，包含一个整数 C，表示有 C 组满足条件的勾股数。

**输入输出样例**

**输入** 

5

**输出**

1

**输入**

13

**输出**

3

**说明/提示：**

*【样例解释 1】* 

*满足 c≤5 的勾股数只有 (3,4,5) 一组。*

*【样例解释 2】*

*满足 c≤13 的勾股数有 3 组，即 (3,4,5)、(6,8,10) 和 (5,12,13)。*

```cpp
#include <iostream>
using namespace std;

int main()
{
    // 定义变量n，用于存储输入的整数
    int n;
    // 从标准输入流中读取整数n
    cin >> n;
    // 定义变量count，用于存储满足条件的整数对的数量
    int count = 0;
    // 外层循环，遍历从1到n的所有整数
    for (int a = 1; a <= n; a++)
    {
        // 中层循环，遍历从a到n的所有整数
        for (int b = a; b <= n; b++)
        {
            // 内层循环，遍历从b到n的所有整数
            for (int c = b; c <= n; c++)
            {
                // 判断是否满足勾股定理
                if (a * a + b * b == c * c)
                {
                    // 如果满足，则计数器加1
                    count++;
                }
            }
        }

    }
    // 输出满足条件的整数对的数量
    cout << count << endl;
}
```

### 三级

#### 3.1 回文字符串

给定一个字符串，请判断它是否为**回文字符串**。回文字符串是指正着读和反着读都相同的字符串，例如 `"abba"`、`"level"` 和 `"12321"` 都是回文字符串。

 **【输入格式】**

- 输入为一个**仅包含英文字母和数字**的非空字符串（不含空格）。
- 字符串长度不超过 `1000`。

**【输出格式】**

- 如果输入是回文字符串，输出 `"YES"`；
- 否则，输出 `"NO"`。

```cpp
#include <iostream> 
#include <string>
using namespace std;

// 判断一个字符串是否是回文
bool isPalindromeString(string str)
{

    // 如果字符串为空，则返回false
    if (str.empty())
    {
        return false;
    }
    // 获取字符串的长度
    int len = str.length();
    // 遍历字符串的前半部分
    for (int i = 0; i < len / 2; i++)
    {
        // 如果字符串的前半部分和后半部分不相等，则返回false
        if (str[i] != str[len - i - 1])
        {
            return false;
        }
    }
    // 如果字符串的前半部分和后半部分相等，则返回true
    return true;
}

int main()
{
    
    // 定义一个字符串变量
    string str;
    // 从标准输入读取一个字符串
    cin >> str;
    // 判断字符串是否为回文
    if (isPalindromeString(str))
    {
        // 如果是回文，输出true
        cout << "true" << endl;
    }
    else
    {
        // 如果不是回文，输出false
        cout << "false" << endl;
    }
}
```
方法二

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string s;
    cin >> s;

    bool is_palindrome = true;
    int left = 0;
    int right = s.length() - 1;

    while (left < right)
    {
        if (s[left] != s[right])
        {
            is_palindrome = false;
            break;
        }
        left++;
        right--;
    }

    if (is_palindrome)
    {
        cout << "YES" << endl;
    }
    else
    {
        cout << "NO" << endl;
    }

    return 0;
}

```



#### 3.2 超级回文字符串

定义一个字符串的奇数长度子串为：以字符串第0号位置开始，长度为奇数的连续子串。例如字符串“abababa”的奇数长度子串有：“a”、“aba”、“ababa”、“abababa”等。

如果一个字符串同时满足以下两个条件：

字符串本身是回文字符串；

以0号位开始的所有奇数长度子串都是回文字符串；

则称该字符串为宇宙无敌回文字符串。

现在，给定一个字符串，判断它是否是宇宙无敌回文字符串。



```cpp
#include <iostream>
#include <string>
using namespace std;

// 判断一个字符串是否回文
bool isPalindrome(const string &s)
{
    int len = s.size();
    for (int i = 0; i < len / 2; i++)
    {
        if (s[i] != s[len - 1 - i])
            return false;
    }
    return true;
}

// 判断从0号位开始的所有奇数长度子串是否都是回文串
bool checkOddLengthSubstrings(const string &s)
{
    int len = s.size();
    for (int subLen = 1; subLen <= len; subLen += 2)
    {
        int start = 0;
        int end = subLen - 1;
        while (start < end)
        {
            if (s[start] != s[end])
                return false;
            start++;
            end--;
        }
    }
    return true;
}

// 判断是否为“宇宙无敌回文字符串”
bool isSuperPalindrome(const string &s)
{
    return isPalindrome(s) && checkOddLengthSubstrings(s);
}

int main()
{
    int n;
    cin >> n;
    while (n--)
    {
        string s;
        cin >> s;
        cout << (isSuperPalindrome(s) ? "Yes" : "No") << "\n";
        
    }
    return 0;
}

```

#### 3.3 带有空格的回文字符串

你想开发一个非常特别的游戏，游戏中的玩家名称(可能包含空格)必须正着读和反着读都一样，否则不能进入这个游戏。为了确保玩家们都能使用自己喜欢的名字，你决定写一个程序来检査给定的昵称是否满足这个要求。(其实就是回文字符串)

**输入格式:**

输入一个昵称，可能包含空格(大小写敏感)。

**输出格式:**

如果昵称是回文，输出“Yes";如果不是回文，输出“No"例如:

**输入:**

l      e       v  e             l

**输出:**

Yes

**输入:**

le vel

**输出:**

yes

**输入:**

leve l

**输出:**

Yes

**输入:**

He11o

**输出:**

No

**提示:**

输入的昵称可以有好多个空格，但是在判断是否是回文时忽略空格。

*代码提示:string newstr="";定义一个空字符串,长度为0,可以把旧的字符串通过循环存进新的字符串里面去,""中间没有空格，有空格这个字符串长度就不是0了!*

```cpp
#include <iostream> // 用于输入输出
#include <string>   // 用于字符串操作
#include <cctype>
int main()
{
    // 优化输入输出，提高效率
    std::ios_base::sync_with_stdio(false);
    std::cin.tie(NULL);

    std::string original_nickname;
    // 使用 getline 读取一整行，包括空格
    std::getline(std::cin, original_nickname);

    std::string processed_nickname = ""; // 用于存储移除空格后的字符

    // 遍历原始字符串，将非空格字符添加到新字符串
    // 使用范围for循环（C++11及以上）
    for (auto c : original_nickname)
    {   
        // 将字符转换为小写，以便不区分大小写
        c=tolower(c);
        // 如果字符不是空格，则添加到 processed_nickname
        if (c != ' ')
        {
            processed_nickname += c;
        }
    }

    // 手动反转 processed_nickname
    std::string reversed_nickname = "";
    // 从 processed_nickname 的末尾开始向前遍历，并添加到 reversed_nickname
    for (int i = processed_nickname.length() - 1; i >= 0; --i)
    {
        reversed_nickname += processed_nickname[i];
    }

    // 比较处理后的字符串和反转后的字符串
    if (processed_nickname == reversed_nickname)
    {
        std::cout << "Yes" << std::endl;
    }
    else
    {
        std::cout << "No" << std::endl;
    }
}
```

#### 3.4 字符串排序

将m个字符串按照以下规则排序：

*1、先按照字符串长度的奇偶性排序：字符串长度是偶数的排在前面，字符串长度是奇数的排在后面。*

*2、对于字符串长度都是偶数的字符串，大写字母字符多的排前面；如果大写字母字符一样多，那么字符串值比较大的排前面。*

*3、对于字符串长度都是奇数的字符串，小写字母字符多的排前面；如果小写字母字符一样多，那么字符串值比较小的排前面。*

把排序后的字符串依次输出出来。

**输入描述**

只有一组案例。

一个正整数m，表示字符串的数量。然后是m个不含空格、'\t'、'\n'的字符串。（m<=100)

**输出描述**

按照指定要求排序后，把排序后的字符串依次输出出来，每个字符串输出完都要换行。

**样例输入**

6

AAA

BBBBB

AAAA

CCCdd

AAaa

ABBA

**样例输出**

ABBA

AAAA

AAaa

CCCdd

AAA

BBBBB

```cpp
#include <iostream>  // 用于输入输出，比如 cin 和 cout
#include <vector>    // 用于 std::vector，存储字符串的容器
#include <string>    // 用于 std::string，处理字符串
#include <algorithm> // 用于 std::sort，排序算法
#include <cctype>    // 用于 std::isupper 和 std::islower，检查字符大小写

int main()
{
    // 优化输入输出，这能让程序在处理大量数据时跑得更快，
    std::ios_base::sync_with_stdio(false);
    std::cin.tie(NULL);

    int m;                               // 声明一个整数变量 m，用于存储字符串的数量
    std::cout << "请输入字符串的数量: "; // 提示用户输入
    std::cin >> m;                       // 从键盘读取一个整数，存入 m

    std::vector<std::string> strings(m); // 创建一个名为 strings 的容器，可以存放 m 个字符串

    std::cout << "请输入 " << m << " 个字符串 (每个字符串输入后按回车):" << std::endl;
    // 循环 m 次，每次读取一个字符串
    for (int i = 0; i < m; ++i)
    {
        std::cin >> strings[i]; // 读取第 i 个字符串
    }

    // --- 核心部分：使用 Lambda 表达式作为排序规则 ---
    // std::sort 需要三个参数：
    // 1. 容器的开始位置 (strings.begin())
    // 2. 容器的结束位置 (strings.end())
    // 3. 比较函数 (Lambda 表达式)

    // 不带自定义比较函数（默认升序排序）：
    // std::sort(起始迭代器, 结束迭代器);
    // 例如：std::sort(my_vector.begin(), my_vector.end());
    // 这种用法会根据元素类型的默认 < 运算符（小于运算符）进行升序排序。对于数字，就是从小到大；对于字符串，就是按字典序从小到大。
    // 带自定义比较函数（可实现升序或降序)
    std::sort(strings.begin(), strings.end(),
              // 这是一个 Lambda 表达式的定义：
              // []           - 捕获列表，这里是空的，表示不从外部捕获变量 问在其定义范围之外（但它能看到的）的变量。
              // 想象一下 Lambda 表达式是一个小盒子，它有自己的参数（像 (const std::string& s1, const std::string& s2)），但有时候它还需要用到它外面的一些数据。捕获列表就是告诉这个小盒子，它需要把哪些外部数据“装”进来，以便在内部使用。
              //[]：不捕获任何外部变量。
              // [var]：按值捕获 var。
              // [&var]：按引用捕获 var。
              // [=]：按值捕获所有在 Lambda 中使用的外部变量。
              // [&]：按引用捕获所有在 Lambda 中使用的外部变量。
              // (const std::string& s1, const std::string& s2) - 参数列表，表示它接收两个字符串 s1 和 s2 进行比较
              // { ... }      - 函数体，包含了我们所有的比较逻辑
              [](const std::string &s1, const std::string &s2)
              {
                //true 表示：s1 应该排在 s2 的前面。
                //false 表示：s1 不应该排在 s2 的前面。
                  bool s1_is_even_len = (s1.length() % 2 == 0); // 判断 s1 长度是否为偶数
                  bool s2_is_even_len = (s2.length() % 2 == 0); // 判断 s2 长度是否为偶数

                  // 规则1：先按照字符串长度的奇偶性排序
                  // 偶数长度排前面，奇数长度排后面。
                  if (s1_is_even_len && !s2_is_even_len)
                  {
                      return true; // s1 是偶数，s2 是奇数，s1 应该排在前面
                  }
                  if (!s1_is_even_len && s2_is_even_len)
                  {
                      return false; // s1 是奇数，s2 是偶数，s2 应该排在 s1 前面 (所以 s1 不应该排前面)
                  }

                  // 如果两个字符串的长度奇偶性相同
                  if (s1_is_even_len&&s2_is_even_len)
                  { 
                      // 规则2：对于偶数长度的字符串
                      // 大写字母多的排前面；大写字母一样多，则字符串值大的排前面。

                      // 计算 s1 中大写字母的数量
                      int uc1 = 0;
                      for (char c : s1)
                      { // 遍历 s1 中的每个字符
                          if (std::isupper(c))
                          { // 如果字符是大写字母
                              uc1++;
                          }
                      }

                      // 计算 s2 中大写字母的数量
                      int uc2 = 0;
                      for (char c : s2)
                      {
                          if (std::isupper(c))
                          {
                              uc2++;
                          }
                      }

                      if (uc1 > uc2) // s1 的大写字母更多
                      {
                         return true; // s1 应该排在 s2 前面
                      }
                      else if (uc1 < uc2) // s2 的大写字母更多
                      {
                         return false; // s1 不应该排在 s2 前面 (即 s2 应该排在 s1 前面)
                      }
                      else // uc1 == uc2，大写字母数量相同
                      {
                         return s1 > s2; // 按照字符串值（字典序）大的排前面
                      }
                  }
                  else if(!s1_is_even_len&&!s2_is_even_len)
                  {   // 此时 s1 和 s2 都 是奇数长度
                      // 规则3：对于奇数长度的字符串
                      // 小写字母多的排前面；小写字母一样多，则字符串值小的排前面。

                      // 计算 s1 中小写字母的数量
                      int lc1 = 0;
                      for (char c : s1)
                      {
                          if (std::islower(c))
                          { // 如果字符是小写字母
                              lc1++;
                          }
                      }

                      // 计算 s2 中小写字母的数量
                      int lc2 = 0;
                      for (char c : s2)
                      {
                          if (std::islower(c))
                          {
                              lc2++;
                          }
                      }
                    if (lc1 > lc2) // s1 的小写字母更多
                    {
                        return true; // s1 应该排在 s2 前面
                        /* code */
                    }
                    else if (lc1 < lc2) // s2 的小写字母更多
                    {
                        return false; // s1 不应该排在 s2 前面 (即 s2 应该排在 s1 前面)

                    }
                    else
                    {
                        return s1 < s2; // 按照字符串值（字典序）小的排前面
                    }
                  }
              } // Lambda 表达式到此结束
    ); // std::sort 函数调用到此结束

    std::cout << "\n排序后的字符串为:" << std::endl;
    // 依次输出排序后的字符串
    for (const std::string &s : strings)
    {
        std::cout << s << std::endl;
    }

    return 0; // 程序成功执行
}
```



#### 3.5 相似字符串

对于两个字符串 A 和 B，如果 A 可以通过删除一个字符，或插入一个字符，或修改一个字符变成 B，那么我们说 A 和 B 是相似的。

比如 apple 可以通过插入一个字符变成 applee，可以通过删除一个字符变成 appe，也可以通过修改一个字符变成 bpple。因此 apple 和 applee、appe、bpple 都是相似的。但 applee 并不能 通过任意一个操作变成 bpple，因此它们并不相似。

特别地，两个完全相同的字符串也是相似的。

给定 T 组 A,B，请你分别判断它们是否相似。

**输入格式**

第一行一个正整数 T。

接下来 T 行，每行两个用空格隔开的字符串 A 和 B。

**输出格式**

对组 A,B，如果他们相似，输出 similar，否则输出 not similar。

**输入输出样例**

**输入**

5

apple applee

apple appe

apple bpple

applee bpple

apple apple

**输出**

similar

similar

similar

not similar

similar

```cpp
#include <iostream>
#include <string>
#include<cmath>
using namespace std;

int main()
{
 
	int n;
	cin>>n;
	for (int i = 0; i < n; i++)
	{
		string a,b;
		cin>>a>>b;
		int alen = a.length();
		int blen = b.length();
		//两个完全相同的字符串也是相似的
		if(a==b)
		{
			cout<<"similar"<<endl;
		}
		//如果两个字符串的长度差大于1，则不相似
		if(abs(alen-blen)>1)
		{
			cout<<"not similar"<<endl;
			//TODO
		}
		//如果两个字符串的长度相等，则比较每个字符是否相等
		else if(alen == blen)
		{
			for (int j = 0; j < alen; j++)
			{
				if (a[j] != b[j])
				{
					//将a中第j个字符替换为b中第j个字符
					a.erase(j,1);
					a.insert(j,1,b[j]);
					//如果替换后两个字符串相等，则相似
					if(a==b)
					{
						cout<<"similar"<<endl;
						break;
					}
					//否则不相似
					else
					{
						cout<<"not similar"<<endl;
					}
				}
			}
		}
		//不相同
		else
		{
			//如果a的长度大于b的长度，则将a中多出的字符删除
			if (alen - blen==1)
			{
				for (int j = 0; j < alen; j++)
				{
					if (a[j] != b[j])
					{
						a.erase(j,1);
						//如果删除后两个字符串相等，则相似
						if(a==b)
						{
							cout<<"similar"<<endl;
							break;
						}
						else
						{
							cout<<"not similar"<<endl;
							break;
						}
					}
				}
			}
			else if(blen-alen==1)
			//如果b的长度大于a的长度，则将b中多出的字符删除
			{
				for (int j = 0; j < blen; j++)
				{
					if (a[j] != b[j])
					{
						b.erase(j,1);
						if(a==b)
						//如果删除后两个字符串相等，则相似
						{
							cout<<"similar"<<endl;
							break;
						}
						else
						//否则不相似
						{
							cout<<"not similar"<<endl;
							break;
						}
					}

					
				}
				/* code */
			}
		}
		
		/* code */
	}
	
}
```

#### 3.6 查找子串

已知由小写字母组成的两个字符串a和b，问a的子串中有多少个也同时是b的子串。在统计过程中，如果遇到a的子串重复多次出现，则算作一个。

**注意事项：**

任意字符串都是自己的子串，空字符串是任意字符串的子串。

**输入描述**

一个正整数n，表示案例的数量。（n<=50）

每组案例中有两个由小写字母组成的字符串a和b。（字符串长度均不大于100）

**输出描述**

针对每组案例，输出一个整数，表示满足条件的子串数量。

每组案例输出完都要换行。

**样例输入**

1

banana ban

**样例输出**

7

**解释：**

*字符串 a 是 "banana"，字符串 b 是 "ban"。*

*首先，我们列出 a ("banana") 的所有不重复子串：*

*长度 0: ""*

*长度 1: a, b, n*

*长度 2: ba, an, na*

*长度 3: ban, ana, nan*

*长度 4: bana, anan*

*长度 5: banan*

*长度 6: banana*

*接下来，我们列出 b ("ban") 的所有不重复子串：*

*长度 0: ""*

*长度 1: b, a, n*

*长度 2: ba, an*

*长度 3: ban*

*现在，我们找出 a 的子串中，哪些也同时出现在 b 的子串列表中：*

*"" (在 a 中，也在 b 中) - 符合*

*a (在 a 中，也在 b 中) - 符合*

*b (在 a 中，也在 b 中) - 符合*

*n (在 a 中，也在 b 中) - 符合*

*ba (在 a 中，也在 b 中) - 符合*

*an (在 a 中，也在 b 中) - 符合*

*ban (在 a 中，也在 b 中) - 符合*

*na (在 a 中，不在 b 中)*

*ana (在 a 中，不在 b 中)*

*nan (在 a 中，不在 b 中)*

*bana (在 a 中，不在 b 中)*

*anan (在 a 中，不在 b 中)*

*banan (在 a 中，不在 b 中)*

*banana (在 a 中，不在 b 中)*

*统计符合条件的子串数量："", "a", "b", "n", "ba", "an", "ban"。*

*总共有 7 个。*

```cpp
#include <iostream>  // 用于输入输出，如 cin, cout
#include <string>    // 用于 string 类
#include <set>       // 用于 set 容器，存储唯一的字符串

// 使用命名空间，方便书写
using namespace std;

// 函数：获取一个字符串的所有唯一子串
set<string> getAllUniqueSubstrings(const string &s)
{
    set<string> substrings;

    // 添加空字符串（根据题目要求，空字符串是任意字符串的子串）
    substrings.insert("");

    // 遍历所有可能的起始位置
    for (int i = 0; i < s.length(); ++i)
    {
        // 遍历所有可能的结束位置 (从当前起始位置到字符串末尾)
        for (int j = i; j < s.length(); ++j)
        {
            // 从 s 的索引 i 开始，长度为 (j - i + 1) 的子串
            substrings.insert(s.substr(i, j - i + 1));
        }
    }
    return substrings;
}

int main()
{
    // 优化输入输出，在处理大量数据时提高效率
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); // cin.tie(NULL) 可以加速 cin 的输入速度
    int n; // 案例数量
    cin >> n;

    // 循环处理每个案例
    while (n--)
    {
        string a, b;
        cin >> a >> b;

        // 1. 获取字符串 a 的所有唯一子串
        set<string> a_substrings = getAllUniqueSubstrings(a);

        // 2. 获取字符串 b 的所有唯一子串
        set<string> b_substrings = getAllUniqueSubstrings(b);

        int count = 0; // 统计符合条件的子串数量

        // 3. 遍历 a 的所有唯一子串，检查它们是否也在 b 的子串集合中
        for (const string &sub : a_substrings)
        {
            // 使用 count 方法在 set 中查找子串是否存在
            if (b_substrings.count(sub))
            {
                count++;
            }
        }

        // 输出结果，每组案例后换行
        cout << count << endl;
    }

    return 0;
}
```

#### 3.7 最多的单词

在文本处理中， 统计单词出现的频率是一个常见的任务。 现在， 给定n个单词， 你需要找出其中出现次数最多的单词。 在本题中， 忽略单词中字母的大小写（即 Apple 、 apple 、 APPLE 、 aPPle 等均视为同一个单词） 。

请你编写一个程序， 输入n个单词， 输出其中出现次数最多的单词。

**输入格式**

第一行， 一个整数 ， 表示单词的个数；

接下来n行， 每行包含一个单词， 单词由大小写英文字母组成。

*输入保证， 出现次数最多的单词只会有一个。*

**输出格式**

输出一行， 包含出现次数最多的单词（输出单词为小写形式） 。

**输入样例** 

6

Apple

banana

apple

Orange

banana

apple  

**输出样例** 

apple

**数据范围**

对于所有测试点，1≤n≤100 ， 每个单词的长度不超过30， 且仅由大小写英文字母组成

方法一：

map容器：

```cpp
#include <iostream>
#include <string>
#include <map>
#include <algorithm>
using namespace std;

// 转小写函数
string toLower(string s)
{
    // 遍历字符串中的每个字符
    for (char c : s)
        // 如果字符是大写字母，则将其转换为小写字母
        if (c >= 'A' && c <= 'Z')
            c += 'a' - 'A';
    // 返回转换后的字符串
    return s;
}
void fun01(int n)
{
        // 定义最多20个单词及其计数
    string w[20] = {""}; // w0 ~ w19
    int cnt[20] = {0};

    // 遍历输入的单词
    for (int i = 0; i < n; ++i)
    {
        string s;
        cin >> s;
        // 将单词转换为小写
        s = toLower(s);

        bool found = false;
        // 遍历已记录的单词
        for (int j = 0; j < 20; ++j)
        {
            // 如果找到相同的单词，则计数加1
            if (w[j] == s)
            {
                cnt[j]++;
                found = true;
                break;
            }
            // 如果没有找到相同的单词，则将新单词记录下来，计数为1
            else if (w[j] == "" && !found)
            {
                w[j] = s;
                cnt[j] = 1;
                found = true;
                break;
            }
        }
    }

    // 找最大值
    int maxIndex = 0;
    for (int i = 1; i < 20; ++i)
    {
        // 如果找到计数更大的单词，则更新最大值
        if (cnt[i] > cnt[maxIndex])
        {
            maxIndex = i;
        }
    }

    // 输出出现次数最多的单词
    cout << w[maxIndex] << endl;
}
void fun02(int n)
{
     // 如果n小于1或者大于100，输出"Invalid input"并返回1
    if (n < 1 || n > 100)
    {
        cout << "Invalid input" << endl;
        return;
    }

    // 定义一个map，用于存储单词和出现次数
    map<string, int> cnt;
    // 定义一个变量，用于存储最大出现次数
    int max_count = -1;

    // 循环n次
    for (int i = 0; i < n; i++)
    {
        // 输入一个字符串s
        string s;
        cin >> s;
        // 如果s的长度大于30，输出"Word too long"并返回1
        if (s.length() > 30)
        {
            cout << "Word too long" << endl;
            return;
        }
        // 将s转换为小写
        //transform 是 C++ STL 中的一个算法，用来对一段范围内的元素逐个进行操作，并写入目标位置。
        //transform(起始位置, 结束位置, 输出位置, 操作函数);
        transform(s.begin(), s.end(), s.begin(), ::tolower);
        // 将s和出现次数存入map中
        cnt[s]++;
        // 更新最大出现次数
        max_count = max(max_count, cnt[s]);
    }

    // 定义一个变量，用于存储出现最大次数的单词数量
    int max_num = 0;
    // 定义一个变量，用于存储出现最大次数的单词
    string result;

    // 遍历map中的所有元素
    for (const auto &p : cnt)
    {
        // 如果某个单词的出现次数等于最大出现次数
        if (p.second == max_count)
        {
            // 将该单词存入result中
            result = p.first;
            // 更新出现最大次数的单词数量
            max_num++;
        }
    }

    // 如果出现最大次数的单词数量为1，输出该单词
    if (max_num == 1)
    {
        cout << result << '\n';
        // 否则，输出"Unexpected: multiple max"
    }
    else
    {
        cout << "Unexpected: multiple max" << endl;
    }
}
int main()

{
    int n;
    cin >> n;
    // 调用函数fun01
    fun01(n);
    // 调用函数fun02
    fun02(n);
}
```

方法二：

字符串静态数组：

```cpp
#include <iostream>
#include <string>
using namespace std;

int toLowerStr(string &s)
{
    // 遍历字符串中的每一个字符
    for (int i = 0; i < s.length(); ++i)
    {
        // 如果字符是大写字母
        if (s[i] >= 'A' && s[i] <= 'Z')
        {
            s[i] = s[i] - 'A' + 'a'; // 转小写
        }
    }
    return 0;
}

int main()
{
    int n;
    cin >> n; // 输入单词数量

    const int MAX = 100;
    string words[MAX];     // 存储单词
    int count[MAX] = {0};  // 存储每个单词的出现次数
    int unique_count = 0;  // 当前不同单词的数量

    for (int i = 0; i < n; ++i)
    {
        string temp;
        cin >> temp; // 输入单词
        toLowerStr(temp); // 转为小写

        bool found = false;
        for (int j = 0; j < unique_count; ++j)
        {
            if (words[j] == temp) // 如果单词已经存在
            {
                count[j]++; // 出现次数加1
                found = true;
                break;
            }
        }

        if (!found) // 如果单词不存在
        {
            words[unique_count] = temp; // 存储单词
            count[unique_count] = 1; // 出现次数为1
            unique_count++; // 不同单词数量加1
        }
    }

    // 找出出现次数最多的单词
    int maxIndex = 0;
    for (int i = 1; i < unique_count; ++i)
    {
        if (count[i] > count[maxIndex]) // 如果当前单词出现次数大于最大出现次数
        {
            maxIndex = i; // 更新最大出现次数的单词索引
        }
    }

    cout << words[maxIndex] << endl; // 输出出现次数最多的单词

    return 0;
}

```



#### 3.8 奇怪的键盘

小苏同学有一个文本编辑器和一个奇怪的键盘。这个键盘有 26 个小写英文字母和退格键(backspace)，一共 27 个键。

每次当她按下任何一个小写英文字母的键的时候，文本编辑器就会在当前编辑文本的末端添加对应的字母。

例如，假设当前文本是 luog，当她按下 u 键时，文本就会变成 luogu。

当她按下退格键的时候，文本编辑器就会删除当前文本的最后一个字母。如果当前文本是空的，则什么都不会发生。

例如，如果当前文本是 luogu，当她按下退格键后，文本就会变成 luog。

现在，给定小苏的按键情况，已知在初始时文本为空，请你求出小苏按完给定的所有键后的文本是什么。

**输入格式**

第一行是一个整数 n（1≤n≤100），表示按键的次数。

第二行是 n 个用空格隔开的字符串，依次表示小苏按下的每个按键。

输入的每个字符串要么是一个小写字母，表示对应的按键，要么是字符串 <bs>，表示退格键。

**输出格式**

输出一行一个字符串，表示小苏按完给定的所有按键后的文本。数据保证输出不是空串。

```cpp
#include <iostream>
#include <string>
using namespace std;

int main()
{
    // 定义一个整数n，用于存储输入的字符串个数
    int n;
    // 从标准输入读取n
    cin >> n;
    // 定义一个字符串s，用于存储输入的字符串
    string s;
    // 定义一个字符串res，用于存储最终的结果
    string res;
    // 循环n次
    for (int i = 0; i < n; i++)
    {
        // 从标准输入读取字符串s
        cin >> s;
        // 如果字符串s等于"<bs>"
        if (s == "<bs>")
        {
            // 如果res不为空，则删除res的最后一个字符
            if (!res.empty())
                res.pop_back();//删除尾部
        }
        // 否则，将字符串s添加到res的末尾
        else
        {
            res += s;
        }

    }
    // 输出res
    cout << res << endl;
    // 返回0，表示程序正常结束
    return 0;
}
```

方法三：

字符串动态数组：

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// 把一个字符串转换成小写
// 将字符串s中的所有大写字母转换为小写字母
void toLowerStr(string &s)
{
    // 遍历字符串s中的每个字符
    for (char &ch : s)
    {
        // 如果字符是大写字母
        if (ch >= 'A' && ch <= 'Z')
        {
            // 将大写字母转换为小写字母
            ch = ch - 'A' + 'a';
        }
    }
}

int main()
{
    int n;
    cin >> n;  // 输入单词个数

    vector<string> words;  // 存储不重复的单词
    vector<int> counts;    // 对应的出现次数

    for (int i = 0; i < n; ++i)
    {
        string temp;
        cin >> temp;  // 输入单词
        toLowerStr(temp);  // 转为小写

        bool found = false;
        for (int j = 0; j < words.size(); ++j)
        {
            if (words[j] == temp)
            {
                counts[j]++;  // 单词已存在，出现次数加1
                found = true;
                break;
            }
        }

        if (!found)
        {
            words.push_back(temp);  // 单词不存在，添加到words中
            counts.push_back(1);  // 出现次数为1
        }
    }

    // 找出出现次数最多的单词
    int maxIndex = 0;
    for (int i = 1; i < counts.size(); ++i)
    {
        if (counts[i] > counts[maxIndex])
        {
            maxIndex = i;
        }
    }

    cout << words[maxIndex] << endl;  // 输出出现次数最多的单词

    return 0;
}

```



#### 3.9 冒泡排序（一维数组）

输入一个包含 `n` 个整数的一维数组，使用**冒泡排序算法**将其按**升序排列**，并输出排序后的结果。

**输入格式：**

第一行输入一个整数 `n`（`1 <= n <= 1000`），表示数组元素个数。

第二行输入 `n` 个整数，表示数组的元素。

**输出格式：**

输出一行，包含排序后的一维数组元素，用空格分隔。

**输入示例：**

9 3 6 2 7

**输出示例：**

2 3 6 7 9

```cpp
#include <iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;

    int a[1000];

    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    // 冒泡排序
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                // 交换
                int temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
            }
        }
    }

    // 输出排序后的数组
    for (int i = 0; i < n; i++) {
        cout << a[i]<< " ";
    }

    cout << endl;
    return 0;
}

```

#### 3.10 密码合规

网站注册需要有用户名和密码，编写程序以检查用户输入密码的有效性。合规的密码应满足以下要求 :。

1. 只能由 a∼z 之间 26 个小写字母、A∼Z 之间 26 个大写字母、0∼9 之间 10 个数字以及 `!@#$` 四个特殊字符构成。

2. 密码最短长度 :6 个字符，密码最大长度 :12 个字符。

3. 大写字母，小写字母和数字必须至少有其中两种，以及至少有四个特殊字符中的一个。

**输入格式**：

输入一行不含空格的字符串。约定长度不超过 100。该字符串被英文逗号分隔为多段，作为多组被检测密码。

**输出格式**：

输出若干行，每行输出一组合规的密码。输出顺序以输入先后为序，即先输入则先输出。

**样例**

**输入**:

```
seHJ12!@,sjdkffH$123,sdf!@&12HDHa!,123&^YUhg@!
```

**输出 **:

```
seHJ12!@
sjdkffH$123
```

**说明/提示**

【样例解释】

输入被英文逗号分为了四组被检测密码：`seHJ12!@`、`sjdkffH$123`、`sdf!@&12HDHa!`、`123&^YUhg@!`。其中 `sdf!@&12HDHa!` 长度超过 12 个字符，不合规；`123&^YUhg@!` 包含四个特殊字符之外的字符不合规。

size_t 是 C 和 C++ 中一个无符号整数类型，专门用来表示对象的大小、数组的下标、内存容量等。你可以理解为是专门为“大小”和“计数”设计的类型

解法一，使用cctype:

```cpp
#include <iostream>
#include <string>
#include <cctype> // 使用 isdigit, islower, isupper 等函数
using namespace std;

// 判断字符是否是允许的合法字符
bool is_valid_char(char ch)
{
    return isalnum(ch) || ch == '!' || ch == '@' || ch == '#' || ch == '$';
}

// 判断一个密码是否合规
bool is_valid_password(const string &pwd)
{
    // 长度必须在 6 到 12 之间
    if (pwd.length() < 6 || pwd.length() > 12) return false;

    bool has_lower = false, has_upper = false, has_digit = false, has_special = false;

    for (char ch : pwd) {
        // 如果有非法字符，直接返回 false
        if (!is_valid_char(ch)) return false;

        // 判断字符类型
        if (islower(ch)) has_lower = true;
        else if (isupper(ch)) has_upper = true;
        else if (isdigit(ch)) has_digit = true;
        else if (ch == '!' || ch == '@' || ch == '#' || ch == '$') has_special = true;
    }

    // 至少有 大小写/数字 中的两种 + 一个特殊字符
    int type_count = (has_lower + has_upper + has_digit);
    return type_count >= 2 && has_special;
}

int main()
{
    string input;
    getline(cin, input); // 获取整行输入（包含多个密码，用逗号分隔）

    string temp; // 存储当前处理的密码
    for (size_t i = 0; i <= input.length(); ++i) {
        // 如果到了末尾或遇到逗号，处理 temp 中的密码
        if (i == input.length() || input[i] == ',') {
            if (!temp.empty() && is_valid_password(temp)) {
                cout << temp << endl; // 输出合法密码
            }
            temp.clear(); // 清空，为下一个密码做准备
        }
        else {
            temp += input[i]; // 拼接字符到当前密码
        }
    }

    return 0;
}

```

解法二，不用cctype：

```cpp
#include <iostream>
#include <string>
using namespace std;

// 判断是否是数字字符
bool is_digit(char ch) {
    return ch >= '0' && ch <= '9';
}

// 判断是否是小写字母
bool is_lower(char ch) {
    return ch >= 'a' && ch <= 'z';
}

// 判断是否是大写字母
bool is_upper(char ch) {
    return ch >= 'A' && ch <= 'Z';
}

// 判断是否是合法字符：数字、小写、大写、特殊符号 !@#$
bool is_valid_char(char ch) {
    return is_digit(ch) || is_lower(ch) || is_upper(ch) || ch == '!' || ch == '@' || ch == '#' || ch == '$';
}

// 判断密码是否合规
bool is_valid_password(const string &pwd)
{
    if (pwd.length() < 6 || pwd.length() > 12) return false;

    bool has_lower = false, has_upper = false, has_digit = false, has_special = false;

    for (char ch : pwd) {
        if (!is_valid_char(ch)) return false;

        if (is_lower(ch)) has_lower = true;
        else if (is_upper(ch)) has_upper = true;
        else if (is_digit(ch)) has_digit = true;
        else if (ch == '!' || ch == '@' || ch == '#' || ch == '$') has_special = true;
    }

    int count = (has_lower + has_upper + has_digit);
    return count >= 2 && has_special;
}

int main()
{
    string input;
    getline(cin, input); // 输入多个密码，英文逗号分隔

    string temp; // 临时存储单个密码
    for (size_t i = 0; i <= input.length(); ++i) {
        if (i == input.length() || input[i] == ',') {
            if (!temp.empty() && is_valid_password(temp)) {
                cout << temp << endl; // 输出合规密码
            }
            temp.clear(); // 重置临时密码
        }
        else {
            temp += input[i]; // 继续拼接字符
        }
    }

    return 0;
}

```

#### 3.11 变成回文字符串

小陈在背英语单词时，特别喜欢回文字符串。一个字符串如果正着读和反着读一样，那么它就是回文字符串。
小陈可以通过修改字符串中的某些字符，使得字符串变成回文字符串。每次可以选择把一个位置上的字母修改成另一个字母。
问:至少需要多少次修改，才能把一个字符串改成回文字符串?
**输入描述:**
一个正整数n，表示有n组案例。
每组案例包含一个不含空格的字符串(大小写敏感)。
**输出描述:**
针对每组案例，输出最少变换次数。
每组案例输出完都要换行。
**样例输入:**
3
ababa
abaaa
aabbbb
**样例输出:**
0
1
2
**解释:**
*ababa是回文字符串，不需要修改。*
*abaaa不是回文字符串，但是可以通过修改最后一个字符，变成回文字符串*
*aabbbb不是回文字符串，但是可以通过修改两个字符，变成回文字符串。*

```cpp
#include <iostream>
#include <string>
using namespace std;

// 计算将字符串变为回文字符串所需的最小修改次数
int minChangesToPalindrome(string s)
{
    int count = 0;
    int len = s.length();

    // 双指针比较左右字符
    for (int i = 0; i < len / 2; ++i)
    {
        if (s[i] != s[len - 1 - i])
        {
            count++; // 只要一对不等，就需要一次修改
        }
    }

    return count;
}

int main()
{
    int n;
    cin >> n; // 输入组数
    string str;

    // 逐组处理
    for (int i = 0; i < n; ++i)
    {
        cin >> str;
        cout << minChangesToPalindrome(str) << endl;
    }

    return 0;
}

```

