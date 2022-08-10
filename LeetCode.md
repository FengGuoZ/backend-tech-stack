### 时间复杂度

>左神基础班 -- 20210919~20211120

**时间复杂度O**：度量**算法执行时间与数据规模关系**的指标

- 不考虑必须要的操作（赋初值、初始化等）

- **只考虑最高阶项**（`O( )`描述的是上限问题）
  - 不考虑低阶项
  - 不考虑最高阶项系数

- 默认规定

  - **数据规模足够大**

  - **同阶复杂度**通过**实验测时**定优劣

  - **学算法时**，只考虑**最坏**时间复杂度

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211108211245058.png" alt="image-20211108211245058" style="zoom:50%;" />



### 01 排序算法🌼🌼 

![image-20220612164517232](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220612164517232.png)

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210922100642697.png" alt="image-20210922100642697" style="zoom: 50%;" />

- 十大排序算法：**选泡插 快归堆希 统计基**
- 排序算法**稳定性**：**相等数据的相对顺序在排序过程中不会变换**
- **基于比较的排序**，时间复杂度最小为**nlogn**
- 桶计基为不基于比较的排序
- 时间复杂度为**nlogn**时，**没有**能同时做到空间复杂度**O(n)以下**和**保持稳定**



#### n^2^排序

##### 1. 选择排序（不稳）🌼🌼

- **最小值移动到头部**

- 算法思路
  - 遍历 **[ii ~ n)** ，记录**最小下标** minPos 
  - `swap(Arr[ii], Arr[minPos])`
    - 每次遍历**只swap一次**
  - **循环 n 次**
- 算法优化：记录**最小与最大下标**，分别移至（相对）开头和（相对）结尾。
  - 使得**循环次数减半**



##### 2. 冒泡排序（稳）🌼🌼

- **最大值冒泡到尾部**

- 算法思路

  - 遍历 **[0 ~ ii]** 

  - 过程中 **swap 逆序对**

    ```c++
    if(arr[jj] > arr[jj+1])
        swap(arr[jj], arr[jj+1]);
    ```

  - **循环 n 次**

- 算法优化：如果某次 **[0 ~ ii]**遍历中，发生**0次**交换，**直接跳出循环**

  - 用flag标志位实现
  - 使得**最佳时间复杂度为O(n)**



##### 3. 插入排序（稳）🌼🌼

- **做到[0, ii）范围上有序**
- 算法思路
  - 遍历**(ii ~ 0]**
  - 过程中**将 ii 上的插入到合适位置**（通过顺序swap实现）
  - **循环 n 次**

- 算法优化：用**标记位（哨兵）**代替swap方法

- 补充说明：插入排序为 **O(n^2)** 排序中**最重要的一种**
  - 选择排序不稳
  - 冒泡排序太慢



##### 4. 希尔排序（不稳）

- **以不同步长序列进行插入排序**
- **平均时间复杂度**为**n^1.3**
- Knuth序列：

![image-20210925002403517](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210925002403517.png)



#### nlogn排序

##### 5. 归并排序（Merge Sort 稳）🌼🌼

```c++
void mergeSort(vector<int>& nums, int left, int right);
```

-  **算法思想** -- 递归
   -  原数组等分
   -  **使左子数组有序**
   -  **使右子数组有序**
   -  **辅助空间merge**左右有序子数组，使整体有序
      -  注意**要有结果导出**

-  **复杂度**
   -  时间：**O(nlogn)**
   -  空间：**O(n)** ，**需要辅助空间**

-  **适用场景**：**要求稳定性**的排序，一般是类对象 
   - java、python语言对象排序内置方法为（改进的）归并排序



##### 6. 快速排序（Quick Sort 不稳）🌼🌼

```c++
void quickSort(vector<int>& nums, int left, int right);
```

- **算法思想** -- 递归
  - 从当前区间中**随机一个值做pivot**（中轴），并将此值**swap至区间尾巴**
  - **做partition**
    - 本质是 **`小于区` 向右推 `等于区` 直到撞上 `大于区`** 
    - **荷兰国旗问题** 
  - **递归**以上过程



- **注意事项** -- **pivot必须是随机值**，否则时间复杂度为O(n^2)



==归并排序是先递归后处理本层==

==快排是先处理本层后递归==



##### 7. 堆排（Heap Sort 不稳）🌼🌼

**堆结构** -- **逻辑上的完全二叉树**

- **节点对应关系**

  - **左孩子**：i.lchild = i*2 + 1
  - **右孩子**：i.rchild = i*2 + 2
  - **父节点**：i.father = (i - 1)/2 （**i=0节点的父节点为i=0，也满足此公式**）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              

- **堆的分类**

  - **大根堆**：头节点为子树**最大值**
  - **小根堆**：头节点为子树**最小值**

- **堆操作**

  - **heap insert**：向堆中**插入数据**

    - **heapsize++**

    - **向上交换**

      ```mysql
      int father = (cur-1) / 1;
      while(arr[cur] > arr[father]){
      	swap(arr[cur], arr[cur.father]);
      }
      ```

  - **heapify**：**修改堆顶后**，**恢复**为大根堆

    ```c
    while(cur < heapsize){
    	int lchild = (2*cur) + 1;
    	int rchild = (2*cur) + 2;
        int maxIdx = cur;
        
    	if(lchild < heapsize)
    		maxIdx = (arr[cur] >= arr[lchild]) ? cur:lchild;
        if(rchild < heapsize)
            maxIdx = (arr[maxIdx] >= arr[rchild]) ? maxIdx:rchild;
        
        if(maxIdx == cur){		// 已到底，或cur为当前子树的最大值
            break;
        } else{					// 调整位置后继续
            swap(arr[cur], arr[maxIdx]);
            cur = maxIdx;
        }
    }
    ```

  - **heap pop**：**删除**堆顶，并恢复堆结构

    - **`swap(arr[0], arr[--heapsize])`**
      - 堆结构**能直接删除的只有堆尾**
    - heapify

- **堆结构比堆排序更重要**



- 堆结构在C++标准库中的实现**(优先级队列)**：
  - 定义大根堆 **`priority_queue<int> bigHeap;`**
  - 定义小根堆**`priority_queue<int, vector<int>, greater<int>> smallHeap;`**
  - **push()**：执行heapInsert操作
  - **pop()**: 删除堆顶值，然后heapify
    - 没有单独的heapify
  - **top()**：获取堆顶值



##### nlogn排序比较🌼🌼

**归并、快排、堆排比较**

- **快排**的**时间**复杂度**常数项最小** **（快）**
- **堆排**的**空间**复杂度最低**（小）**
- **归并排序**能保持**稳定性** **（稳）**



#### 不基于比较排序

##### 8. 计数排序（稳）🌼🌼

- 算法思想
  - 找出数组中的最大值、最小值
  - 创建计数数组count[max-min+1]
  - 统计数组范围中各个数出现的次数
  - 根据count[max-min+1]还原出排序后的数组
- 算法时间复杂度O(n+k)，空间复杂度O(k)
  - **k为数组数据取值范围**
- **适用于k较小的数组**
  - 例如：人的年龄0-200足矣



##### 9. 基数排序（稳）

- **算法思想**
  - **找出数组最大值，**计算位数k
  - 创建基数**队列** **`queue<int> qArr[10];`**（十进制数每位上的数字必为0~9中的一个）
  - 按照个、十、百、千......k位上的数字，将数组中的值在qArr中归类
  - 将qArr中归类后的数据，赋值给数组，排序完毕
- 队列的**FIFO特性保证了算法的稳定性**
- 时间复杂度O(n*k)，空间复杂度O(n+k)



##### 10. 桶排序（稳）

- **计数排序的拓展版本**
- 计数排序的每个桶只存储相同元素，而桶排序每个桶存储一定范围的元素
  - **桶内部也需要排序**



#### 排序延伸问题

##### 1. 逆序对问题（Merge Sort延伸）51🌼🌼

![image-20220809150447431](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809150447431.png)

- **解题思路**：**归并排序过程中统计**

  ```c++
  int cnt=0, tc=0;
  while(ii<=mid && jj<=right){
      if(nums[ii] <= nums[jj]){       // 必须ii优先占位
          hv[index++] = nums[ii++];
          cnt += tc;                  // 计数位置★
      }else{
          hv[index++] = nums[jj++];
          tc++;                       // 右子数组的逆序基数++
      }
  }
  ```

- **注意事项**：递归+归并过程**能保证不重不漏**



##### 2. 小和问题（Merge Sort延伸）🌼🌼

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220213190746434.png" alt="image-20220213190746434" style="zoom:50%;" />



- 解题思路:**归并排序过程中统计**，思路同上，注意实现细节

  ```c++
  int cnt=0, tc=right-mid;
  while(ii<=mid && jj<=right){
      if(nums[ii] < nums[jj]){        // 必须jj优先占位
          hv[index++] = nums[ii++];
          cnt += tc*nums[ii];         // 小和计算位置★
      }else{
          hv[index++] = nums[jj++];
          tc--;                       // 右子数组的小和基数--
      }
  }
  ```

  

##### 3. 荷兰国旗问题（Quick Sort延伸）75🌼🌼

- **题目描述**
  - 给定一个数组arr，和一个数pivot。
    - 请把小于pivot的数放在数组的左边，
    - 等于pivot的数放在数组的中间，
    - 大于pivot的数放在数组的右边。
  - 要求额外空间复杂度0(1)，时间复杂度0(N)



- **解题思路**

  - 维护三个指针 lb cur rb

    - lb：小于区的右边界，初始化为-1
    - cur：当前位置，初始化为0
    - rb：大于区的左边界,，初始化为nums.size()

  - 本质是 **`小于区` 向右推 `等于区` 直到撞上 `大于区`** 

    ```c++
    // 当前区间[left, right]
    int lb=left-1, rb=right+1, ii=left;         // 初始化为非法位置
    while(ii < rb){
        if(nums[ii] < pivot){
            swap(nums[ii++], nums[++lb]);
        } else if(nums[ii] == pivot){
            ii++;
        } else{	// nums[cur]>pivot
            swap(nums[ii], nums[--rb]);
        }
    }
    // 本次排序完成后 [left, lb]	[lb+1, rb-1]	[rb, right]
    //			     <pivot		   ==pivot		  >pivot
    ```



### 02 链表专题🌼🌼

#### 1.  反转单向、双向链表 206🌼🌼

![image-20220809153853726](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809153853726.png)

**算法思路**

- 单向链表：**用双辅助节点prev、cur**
- 双向链表：单辅助节点cur，swap(next, prev)即可



#### 2. 有序链表的公共部分🌼

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210927223658157.png" alt="image-20210927223658157" style="zoom:50%;" />

**算法思路**

- 两个指针指向头
- 谁小谁后移
- 相同则打印，并同时后移
- 其中一个指针为nullptr时停止

```c++
ListNode *p1=head1, *p2 head2;
while(p1 && p2) {
    if(p1->value == p2->value){
        res.push(p1->value);
        p1 = p1->next;
        p2 = p2->next;
    } else if(p1->value < p2->value){
        p1 = p1->next;
    } else{
        p2 = p2->next;
    }
}
```



#### 3. 分隔链表 86🌼🌼

![image-20220809154854274](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809154854274.png)

**算法思路**

- version1：将链表中的元素放入数组中，**做parition**
  - **没有稳定性**
- version2：用sH、sT、eH、eT、bH、bT**六个指针** **划分出小于pivot、等于pivot、大于pivot三个链表**，而后拼接
  - **稳定**
  - 拼接步骤考验coding能力



#### 4. 求链表入环节点  22🌼🌼

![image-20220809155245418](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809155245418.png)



**算法思路**

- version1：哈希表存储遍历过的节点，第一个重复出现的node为入环node
- version2：**快慢指针** + 快慢指针第一次重合后将快指针归head，第二次重合即为**入环节点**（**魔性的数学**）
  - **归位后，快指针和慢指针每次都只移动一步**



#### 5. 两个单链表相交（链表难度天花板）52🌼🌼

![image-20220809155537897](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809155537897.png)

**算法思路**

- 先判断L1、L2有无环

- 分情况讨论

  |  L1  |  L2  |          判断相交          |
  | :--: | :--: | :------------------------: |
  | 无环 | 无环 | 交换跑道（对的人总会相遇） |
  | 无环 | 有环 |       **不可能相交**       |
  | 有环 | 有环 |        哈希缓存去做        |

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809161930906.png" alt="image-20220809161930906" style="zoom:50%;" />

```c++
// 无环链表相交
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    if(headA==NULL || headB==NULL)
        return NULL;

    ListNode* p1=headA, *p2=headB;
    while(p1 != p2){                    // 无论是否相交都一定会有p1=p2，不会造成死循环
        p1 = p1 ? p1->next : headB;     // 后移1位or交换跑道
        p2 = p2 ? p2->next : headA;     
    }

    return p1;
}
```



#### 6. 删除链表倒数第k个节点 21🌼

![image-20220809163833670](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809163833670.png)

**算法思路：快慢指针**

- fast指针先走**k+1**步

- 特殊情况下，链表共k个节点 - 删除头节点



#### 7. 删除链表节点O(1)  237🌼

![image-20220809164044878](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809164044878.png)



算法思路：trick（**李代桃僵**）

- 赋值下一节点（本节点原内容被覆盖/删除）
- 删除下一节点



### 03 二叉树专题🌼🌼

#### 1.  二叉树的遍历 144 94 145🌼🌼

**实现二叉树的前中后顺遍历**

- 先序遍历：根节点 **->** 左子树 **->** 右子树
- 中序遍历：左子树 **->** 根节点 **->** 右子树
- 后序遍历：左子树 **->** 右子树 **->** 根节点
- **深度优先遍历(DFS)**：即二叉树的先序遍历
- **宽度优先遍历(BFS)**：按层遍历（也叫广度优先遍历、层序遍历）



**递归解法**

- **递归序**：递归过程中，**二叉树的每个节点经过三次**
  - **第一次经过**时记录：**先序**
  - **第二次经过**时记录：**中序**
  - **第三次经过**时记录：**后序**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211003164413091.png" alt="image-20211003164413091" style="zoom:50%;" />



**迭代解法（栈结构）**

- **先序遍历**

  - 压入根节点

  - 栈顶出栈并打印，**先后压入栈顶的右、左孩子**

  - 重复上一步，直至栈为空

- **后序遍历**

```c++
// Talk is cheap, show me the code

class Info{	// 辅助信息结构体
public:
    TreeNode* node;
    int flag;   // 出栈权限 1表示当前不可以出栈 2表示当前可以出栈
    Info(TreeNode* n, int f)
        :   node(n), flag(f) {}
};

vector<int> postorderTraversal(TreeNode* root) {
    if(root == NULL)
        return {};
    
    vector<int> res;
    stack<Info> hSta;
    hSta.push(Info(root, 1));   // root节点第一次入栈 初始权限为不可出栈

    while(!hSta.empty()) {
        if(hSta.top().flag == 2) {  // 当前节点可以出栈
            res.push_back(hSta.top().node->val);
            hSta.pop();
            continue;
        }

        hSta.top().flag = 2;    // 修改top()节点出栈权限

        // 先右后左入栈
        TreeNode* pNode = hSta.top().node;
        if(pNode->right) {
            hSta.push(Info(pNode->right, 1));
        }
        if(pNode->left) {
            hSta.push(Info(pNode->left, 1));
        }
    }

    return res;
}
```

- **中序遍历**（提供了一种**按 / 分解二叉树的思路** ）

  - **压入根节点及所有左子节点**

  - 栈顶出栈并打印，**压入栈顶的右孩子及其所有左子节点**

  - 重复上一步直至栈空


<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211003200903335.png" alt="image-20211003200903335" style="zoom:50%;" />

#### 2. 二叉树每层的最大值 44

![image-20220809170554650](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809170554650.png)

**算法思路**

- 用queue进行层序遍历
- **同时维护层边界**

```c++
queue<TreeNode*> que;
que.push(root);

while(!que.empty()) {
    int levelNum = que.size();  // 用levelNum维护层边界
    int levelMax = INT_MIN;
    
    while(levelNum--) {
        TreeNode* pNode = que.front();
        que.pop();
        if(pNode->left) que.push(pNode->left);
        if(pNode->right) que.push(pNode->right);
    }
}
```



#### 3.  判断BST（二叉排序树）98🌼🌼

**中序序列递增**



#### 4. 判断平衡二叉树 BBT 55🌼🌼

**二叉树递归套路**

左子树为平衡二叉树 && 右子树为平衡二叉树 && 左右子树高度差<=1

```c++
typedef pair<int, bool> Info;	// first表示树高度，second表示是否平衡
Info process(TreeNode* root); 	// 递归接口
```



#### 5. 二叉树的最低公共祖先 236🌼🌼

**二叉树递归套路**

**有且只有**最低公共祖先满足以下条件中的一个

- 左子树包含一个node、右子树包含另一个node
- 自身为一个node，左、右子树中包含另一个node

```c++
int process(TreeNode* root);	// 递归接口，int表示root上目标节点个数
```



#### 6. 二叉树的直径

![image-20220809203700645](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809203700645.png)

**二叉树递归套路**

当前树的最大直径**有3个候选**

- 左子树最大直径
- 右子树最大直径
- 左子树深度+右子树深度+1

```c++
class Info {
public:
    int depth;
    int diameter;
    Info(int d, int dia)
        : depth(d), diameter(dia) {};
};
```





#### . 二叉树的序列化和反序列化 37🌼🌼

![image-20220809202652011](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809202652011.png)

**序列化**：从一棵二叉树转化为字符串

**反序列化**：将字符串还原为二叉树

要求：二叉树与字符串是严格的11映射关系



**算法思路**

- 序列化和反序列化遵循**同一规则**
- 用先序序列化，在反序列化时的规则：**建根节点 -> 建左子树 -> 建右子树**
  - 用**`_`**分隔节点（字符串中存储的是节点值）
  - 用**`#`**标志NULL节点



**注意事项**

- 定制stoi，省去内存拷贝

```c++
// 定制stoi, 省去内存拷贝
int stoi(string& s, int& index){
    int flag = 1;
    if(s[index] == '-') {           // 符号位
        flag = -1;
        index++;
    }
    int res = 0;
    while(index<s.size() && s[index]>='0' && s[index]<='9'){
        res = res*10 + (s[index++] - '0');
    }
    return flag * res;
}
```







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



### ACM指北

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



##### 数组组数位置 -- while读取

```c++
vector<int> arr;
int val = 0;
while(cin >> val) {		// 读取完成后会自动跳出
    arr.push_back(val);
}
```



##### 按行读取

```c++
#include<sstream>	// istringstream头文件

string s;
while(getline(cin, s)) {	// 读取一行
    istringstream line(s);	// string转化为流
    while(line >> val) {
        // ...
    }
}
```





#### 自定义数据结构

```c++
// 链表
struct ListNode {
    int val;
    ListNode *next;
};

// 二叉树
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
};
```



#### Tips

##### 95%通过

很有可能是数据范围的问题

int->long或能解决



##### trick

时间不足时，bool类型直接返回true，可能通过50%的测试用例
