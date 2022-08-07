











### Coding心法

#### 1 条件反射

|     题干输入     |         大脑反射输出         |                补充说明                |
| :--------------: | :--------------------------: | :------------------------------------: |
|   ==有序数组==   | 二分查找<br />双指针顺序夹逼 |               34<br />57               |
|     ==逆序==     |            栈结构            |                                        |
|     ==DFS==      |            栈结构            |          树、图的深度优先遍历          |
|     ==BFS==      |             队列             |          树、图的广度优先遍历          |
|     ==贪心==     |            堆结构            |                                        |
|    ==大数据==    |           哈希分流           |      哈希函数能做到**按种类均分**      |
|  ==二叉搜索树==  |         中序序列递增         |                                        |
| ==删除链表节点== |           伪头节点           |                   19                   |
|   ==二维数组==   |            图结构            | vector<vector\<int>>**提供图的边信息** |
|                  |                              |                                        |



#### 2 建立**严格表时**，建议**用 vector 预分配空间**

注意**二维形式**的 **vector 构造方式**

数组形式不稳定，**编译器容易抽风**

- 编译器可能报错，error: variable-sized object may not be initialized

```c++
int dp[m][n] = {0};     // 建立严格表，编译器不通过
vector<vector<int>> dp(m, vector<int>(n));  // STL一定可以通过

vector<int> chat(10);
vector<vector<int>> chat(10, vector<int>(10))；
vector<vector<vector<int>>> chat(10, vector<vector<int>>(10, vector<int>(10)));
```



#### 3 vector支持==运算符

**通过重载operator==实现**

两个vector内**存储内容完全相同，则返回true**



#### 4 自定义priority_queue比较器

采用类比较器

**第2个参数固定为vector<T>，用于指定存储方式**

```c++
class greaterCmp{	// 自定义类比较器
public:
    bool operator()(pair<int, int> a, pair<int, int> b){
        return a.first*b.second > a.second*b.first;
    }
};

priority_queue<pair<int, int>, vector<pair<int, int>>, greaterCmp> helpPQ;
```



#### 5 引用传参对效率的影响

**在递归函数中，尽量采用引用传参**，效率显著提升

- 因为递归调用过程中会反复传参，此时值传参内存拷贝代价过大
- 同时要注意引用参**对逻辑的影响**，**可能会多一步返还使用权、路径清除操作** （剑指offer 12、34）

![image-20211203220915829](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211203220915829.png)

![image-20211203220950845](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211203220950845.png)



**在比较器中，尽量采用引用传参**，效率显著提升

因为比较器调用过程中会反复传参，此时值传参内存拷贝代价过大

```c++
::sort(courses.begin(), courses.end(), Cmp());	// Leetcode 630 课程表 III
```

![image-20211214223037747](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211214223037747.png)

![image-20211214223141122](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211214223141122.png)



#### 6 递归超时

递归体中，**尽量减少递归的调用次数**，否则容易引起超时

```c++
double temp = myPow(x, n/2);        					// 临时变量存储递归输出，减少递归调用次数
return (n % 2 == 0) ? temp * temp : temp * temp * x;

return myPow(x, n/2) * myPow(x, n - n/2); 				// 会超时
```



#### 7 -int 与 abs(int)

**危险操作，容易越界**

可以转化为更高范围的long进行处理

```c++
double myPow(double x, int n) {
    if(x == 0){
        return 0;
    }
    long a = n;    // int 转 long，否则 -n 时会报错
    return (n > 0) ? process(x, a) : 1.0/process(x, -a);
}
```



#### 8 %取模运算法则

- **和的模** = **分别模的和再模**

- 积的模 = 分别模的积再模
- a的n次方的模 = a的模的n次方再模

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211206015326976.png" alt="image-20211206015326976" style="zoom: 40%;" />



#### 9 逻辑运算符的短路性质 

- A && B，**当A为false 时，B自动跳过**，不会被执行

- A || B，**当A为true 时，B自动跳过**，不会被执行


```c++
if(n<0 || n>=str.size() || str[n]){	// 由于短路性质，这里的 str[n] 不用担心越界问题
	// ...
}
```



#### 10 while循环的结束条件为判断条件刚好越界



#### 11 互斥条件分支必只执行其中一个



#### 12 慎用memse等内存操作API

内存操作**以字节为单位填充**，当元数据类型不为char时

- 除了置0，**其余置位可能发生逻辑错误**
- 记得 × `sizeof(type)`

```c++
int a[10];
memset(a, 1, 10 * sizeof(int));	// 逻辑错误，实际上置位为0x0000 0001 0000 0001 0000 0001 0000 0001 = 16843009
```



#### 13 STL完美支持指针类型

使用STL模板容器时，**任意类型指针可以作为T传入**

- unordered_map等关联式容器**存储自定义类时，建议使用类指针**
- 直接使用自定义类会不支持



#### 14 前缀和数组优化 

面对需要**重复计算数组区间和**的场景，可用前缀和数组优化，提高效率（LeetCode 1856）

```c++
// 计算nums对应的前缀和数组 preSum
vector<long long> preSum(nums.size(), 0); 
for(int ii=0; ii<nums.size(); ii++){
    if(ii == 0)
        preSum[ii] = nums[ii];
    else
        preSum[ii] = preSum[ii-1] + nums[ii];
}

// 计算[ii, jj]区间和
long long ans = preSum[jj] - preSum[ii] + nums[ii]; // 不用ii-1，防越界
```



#### 15 学会适应计算机语言

存在**算法的自然语言表述复杂，但程序表达起来流畅自然的情况**

Don't translate, be origin.



#### 16 关于词频统计

**哈希表通用**，有时可**退化为数组、位图**

看起来用数组、位图可优化常数时间，**但实测二者的时空开销相近**，充分说明了 STL 的高效性



#### 17 换位比较优于对齐比较 

- 字符串长度不一致时，**自然想到的是将较短的一方补齐，统一为等长字符串比较问题** 

- 但这样做带来了**补齐规则难处理**的问题

- 更优的做法是，**比较 a+b 与 b+a**，采用换位操作**自动保证了长度一致**（剑指offer 45）


```c++
bool operator()(const string& a, const string& b){
    return stod(a+b) < stod(b+a);	// 长度自动对齐
    // return a+b < b+a;			// 本题逻辑下，与上一句等价
}
```



#### 18 while循环中容易忘记移位

for循环中i++作为表达式的一部分不容易遗漏

**while循环中常会忘记进行移位操作，导致死循环超时**（剑指offer 67）

```c++
long long res = 0;
while(index<str.size() && str[index]>='0' && str[index]<='9')
    res = res*10 + (str[index++]-'0');  
    index++;			// ++移位,否则进入死循环,本题表现为一定返回越界值INT_MIN
	if(-res < INT_MIN) 	// 越界处理
        return INT_MIN;
}
return -res;
```



#### 19 << 防溢出

最高位置0后再<<，防止溢出，编译器报错

```c++
int val = -1;		
val << 1;				// 溢出报错
(val & INT_MAX) << 1;	// 最高位置0后再左移
```



#### 20 防越界

**`p->next`**前有**`if(p != NULL)`**

**`arr[index]`** 前有 **`if(index < arr.size() && index >= 0)`**

**`stack.pop()`**前有**`while(!stack.empty())`**



易错示例：虽然进入循环时判定过没越界，但在运行过程中，**改变下标后没有再次判定合法性**

```c++
while(row<n && col>=0){
    if(matrix[row][col] > target)   // 左移，有可能导致后续越界
        col--;
    if(matrix[row][col] < target)   // 下移
        row++;
    if(matrix[row][col] == target)
        return true;
}
```



#### 21 二叉树的还原

**还原方式**

- 先序 + **中序**（可以）
- **中序** + 后序（可以）
- 先序 + 后序（**不可以**）

**必须有中序序列**，通过**中序序列+根节点**，可以划分出左右子树，然后递归

以上均要求**节点值不重复**，例如2、2、2的序列有多个答案



#### 22 二叉树的递归套路（即二叉树的**树型DP**）

- **左子树如何**？向左子树要答案

- **右子树如何**？向右子树要答案

- **根节点如何**？向根节点要答案

**根据所需信息建立信息类(class Info)，作为递归返回值**

```c++
class Info{
public:
	int depth;
    int maxVlue;
    Info(int depth_, int maxValue){
        depth = depth;
        maxValue = maxValue;
    }
};

Info foo(...){
	Info ans;
	// ...
    
    return ans;
}
```



#### 23 二维矩阵映射至一维

**value = ii * COL + jj**

可在**矩阵遍历时用作轨迹记录**，标识来过此位置

```
0 0 0 0		0  1  2  3
0 0 0 0	->	4  5  6  7	
0 0 0 0		8  9  10 11
```



#### 24 O计算

计算时间复杂度时，将 **`1 2 3 4 … n-1 n`** 序列等价于 **`n n n n … n n`** 序列



### ACM IO指北

#### IO函数

##### std::cin

cin >> 变量，可自动跳过\n

cin >> string，读取一条字符串

- **以空格或\n为一次读取结束标志**
- 自动跳过个1~多个空格或\n



##### getchar()

读取一个字符

会读入换行符\n和文件结束符EOF



##### getline()

读取一行

```c++
string s;
getline(cin, s);		// 读取完整一行，不会因为空格而暂停
```



##### 判断EOF

getline()

cin

```c++
string s;
while(getline(cin, s)) {	// 读取到EOF时自动跳出
	// ...   
}

while(cin >> s) {			// 读取到EOF时自动跳出
    // ...
}
```



#### 常用IO模式

##### 已知数据组数 -- for读取

```c++
int n = 0;
cin >> n;	// 得到数组组数
vector<int> arr(n);
for(int ii=0; ii<n; ii++) {
    cin >> arr[ii];
}
```



##### 数组组数位置 -- while读取3

```c++
vector<int> arr;
int val = 0;
while(cin >> val) {		// 读取完成后会自动跳出
    arr.push_back(val);
}
```

