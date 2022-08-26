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



#### 7. 对称二叉树（递归接口转化）28🌼🌼

![image-20220811225659972](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220811225659972.png)

思维上**适应递归在逻辑上的压栈行为**



#### 8. 二叉树的序列化和反序列化 37🌼🌼

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



#### 9. 重建二叉树 07

![image-20220811225839485](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220811225839485.png)

**算法思路**

- 利用前序首元素+中序序列划分左右子树
- 递归处理
- 递归过程中，**前序序列和中序序列一直对应同一棵子树**

```c++
// 递归做法，用坐标边界作为递归参数
// 前序序列范围：[pL, pR] 中序序列范围[iL, iR]
TreeNode* process(vector<int>& preorder, vector<int>& inorder, int pL, int pR, int iL, int iR) {   
    
}
```



### 04 树专题

#### 1. 单元最短路径（Dijkstra）算法

**题目描述**：给定一个图和出发节点，找到这个节点到所有节点的最短路径，图中不存在权值为负数的边

![image-20220630113253048](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220630113253048.png)

**算法思路**

- 找到目前未确定节点中的最短路径
- 将该路径对应节点加入已确认集合
- 用该节点的边信息更新代价表
- 重复以上步骤，直至所有节点加入已确认集合



#### 2. 前缀树（Trie） 208

**题目描述**：构建前缀树结构，用于记录字符串集合信息，并支持以下操作

- **insert**：加入word记录
- **prefixNumber**：以指定prefix为前缀的word加入过几次
- **search**：查询指定word之前加入过几次
- delete：删除word的一条记录



**算法思路**

- 前缀树**本质为26叉树**
- 前缀树**root为空节点**
- 前缀树**将字符存储在边上**
- 建树时，有则复用，没有则新建

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220725225435622.png" alt="image-20220725225435622" style="zoom: 67%;" />



**解题代码**

- TireNode结构设计

```c++
class TireNode {
public:
    TireNode() {
        pass = 0;
        end = 0;
        memset(nexts, 0, sizeof(TireNode *) * 26); // 指针清零，新节点没有通向任何字符的路
    }
    int pass;            	// 有多少路径通过该节点
    int end;             	// 有多少路径以该节点结束
    TireNode *nexts[26];	// null表示无该路径，合法节点表示存在后续的指定字母（字母存储在路径上）
};
```



**注意事项**

- 前缀树适用场景
  - **单词查询**
  - 拼写检查
  - 自动补全
- 当前缀树字符集不满足只包含小写字母时：
  -  用map记录当前节点已存在的后续字母 `unordered_map<char, TireNode *> nexts;`



#### 3. 哈夫曼树

哈夫曼树(Huffman Tree)**是带权路径长度最短的二叉树树**，也称最优二叉树

- 输入：N个有权值的叶子结点
- 输出：构造一棵带权路径长度最小的二叉树

- 哈夫曼树**通过堆结构构建**

**哈夫曼编码**

- 一种基于哈夫曼树的**不等长编码**
- 可用于数据压缩



![image-20220812165512394](C:\Users\ChiGUI\AppData\Roaming\Typora\typora-user-images\image-20220812165512394.png)



### 05 图专题

图结构关键子在于如何根据题目需求，表达Node、Edge、Graph

图的Edge信息一半以`vector<vector\<int>> input`格式输入 



#### 1. 图的遍历

DFS -- 用stack实现

BFS -- 用queue实现



#### 2. 拓扑排序 210

![image-20220812002624299](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812002624299.png)

**图的拓扑排序**

- 有向图
- 存在入度为0的节点
- 无环（避免死锁）



**算法思路**

- 解锁入读为0的节点
- 擦除节点影响（可能产生新的入读为0的节点）
- 重复以上步骤

```c++
// 图节点结构
class Node {
public:
    int val;
    unordered_set<int> pre;    // 前置课程数组
    unordered_set<int> next;   // 后继课程数组
};

Node graph[numCourses];	// 图本身
```



### 06 单调栈

#### 1. 单调栈理论

**单调栈**：**从栈底到栈顶**，**所有元素单调**

- **递减单调栈**：栈底到栈顶**递减**

  - **维护方法**：**入栈时**，弹出比当前入栈元素小的元素
  - 维护过程中可以获得有用信息，例如**获得最近左右更大值**

- 递增单调栈

  <img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220422225421267.png" alt="image-20220422225421267" style="zoom: 67%;" />



#### 2. 左右最近更大值位置

**题目描述**

- 获取数组每一个位置的**左右最近更大值位置**（越界标志位为 `-1` ）

- 输入大小为 n 的数组Arr
- 输出大小为 n 的 `vector<pair<int, int>> biggerPos`向量



**算法思路**：**单调栈结构**（以数组**不含重复值为例**）

- 数据依次入栈 `stack<int>`
- 入栈会将**小于**当前元素的数据**弹出**
- **弹出时记录该位置对应信息**
  - **当前待入栈元素**的**左侧最近更大值**为当前**栈顶**
    - 如果栈为空，标志为 -1
  - **被弹出元素**的**右侧最近更大值**为当前**待入队元素**
- 遍历完成后，需对栈中剩余数据进行**收尾处理**
  - **左侧最近更大值**为弹出后的**栈顶**
  - **右侧最近更大值**为 **-1**（没有右侧更大值）

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211102223534299.png" alt="image-20211102223534299" style="zoom: 67%;" />

此题中**单调栈中存储的是下标值**



#### 3. 每日温度 739

![image-20220812004555885](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812004555885.png)

**算法思路**

- **要找右侧最近更大值**

- 应用递减单调栈



#### 4. 子数组最小乘积的最大值 1856

![image-20220812005627703](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812005627703.png)



**算法思路** -- 最终答案**必定从某个以arr[ii]为最小值的子数组**中产生

- **要找左右相邻更小值位置** -- 应用递增单调栈
- 利用左右相邻更小值位置信息，**计算所有i位置对应的f(i)**
- 在所有f(i)中取最大



#### 5. 柱状图中的最大矩形84 

![image-20220812011408963](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812011408963.png)

**算法思路** -- 最终答案**必定从以某个arr[ii]为高的矩形中产生**

- **要找左右相邻更小值位置**
- 应用递增单调栈



#### 6. 最大矩形85

![image-20220613160646856](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220613160646856.png)

**算法思路**

- 复用84题结论
- 将85题分解为N个84题求解（N为行数）



### 07 双指针

**双指针分类**

- 双指针（普通）
- **快慢指针** -- 链表中应用较多
- **双指针顺序夹逼** -- 和有序条件绑定



#### 1. 删除链表倒数第k个节点（快慢指针） 21🌼

![image-20220809163833670](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809163833670.png)

**算法思路：快慢指针**

- fast指针先走**k+1**步

- 特殊情况下，链表共k个节点 - 删除头节点



#### 2. 求链表入环节点（快慢指针）  22🌼🌼

![image-20220809155245418](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220809155245418.png)



**算法思路**

- version1：哈希表存储遍历过的节点，第一个重复出现的node为入环node
- version2：**快慢指针** + 快慢指针第一次重合后将快指针归head，第二次重合即为**入环节点**（**魔性的数学**）
  - **归位后，快指针和慢指针每次都只移动一步**



#### 3. 两个无环链表的第一个公共节点（双指针） 52🌼

![image-20220812132342454](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812132342454.png)

**算法思路**

- 错的人一定错过，**对的人终将重逢**。

```c
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    ListNode* p1 = headA;   // 双指针
    ListNode* p2 = headB;
    while(p1 != p2){
        p1 = (p1==NULL) ? headB : p1->next; // 切换跑道
        p2 = (p2==NULL) ? headA : p2->next; // 切换跑道
    }
    return p1;
}
```

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211201123417644.png" alt="image-20211201123417644" style="zoom:50%;" />



#### 4. 和为s的两个数字（双指针顺序夹逼）57🌼🌼

![image-20220812132818018](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812132818018.png)

**算法思路**

- 双指针顺序夹逼
- LL RR指针**从数组两端向中间夹逼**



#### 5. 统计有序矩阵中的负数（双指针顺序夹逼）1351🌼

![image-20220812134151406](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812134151406.png)

**解题思路**：**双指针顺序夹逼**

- 从右上角->左下角开始顺序夹逼
- **±分界线呈阶梯型**



### 08 二分查找🌼

#### 1. 二分理论🌼

==while <= 搭配LL=mid+1 RR=mid-1==

出循环后，LL为破坏条件的第一个元素

![image-20220417171034282](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220417171034282.png)



#### 2. lower_bound & upper_bound 34🌼

![image-20220812161627628](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812161627628.png)



**算法思路** -- **二分查找**

- **lower_bound**：**>=target**的第1个下标
- **upper_bound**：**>target**的第1个下标
- targert出现的**次数** = **`upper_bound - lower_bound`**



**拓扑关系演示**

![image-20220417172600655](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220417172600655.png)



#### 3. 旋转数组的最小数字 11🌼

![image-20220817151513271](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220817151513271.png)

**算法思路**

- 与传统二分不同，mid与rb比较
- 移位方式kiwi，**强记**

```c++
int minArray(vector<int>& numbers) {
    int lb=0, rb=numbers.size()-1;
	
    while(lb < rb){
        int mid = lb + (rb-lb)/2;				// mid与rb比较，注意移位方式
        if(numbers[mid] < numbers[rb]){
            rb = mid;
        } else if(numbers[ii] > numbers[rb]){
            lb = mid+1;
        } else{
			rb--;
        }
    }
    
    return numbers[rb];
}
```



#### 4.  搜索旋转排序数组 33

![image-20220817153314608](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220817153314608.png)

**算法思路**

`[          part1           ][       part2         ]`

- 分情况二分，首先判断mid归属的part

```c++
int search(vector<int>& nums, int target) {
    int LL = 0, RR = nums.size() - 1;
    while(LL <= RR) {
        int mid = LL + (RR - LL) / 2;
        if(nums[mid] == target) {
            return mid;
        }
        if(nums[LL] < nums[mid]) {  // mid位于part1中
            if(nums[LL] <= target && nums[mid] > target) {
                RR = mid - 1;
            } else {
                LL = mid + 1;
            }
            continue;
        }
        if(nums[mid] < nums[RR]) {  // mid位于part2中
            if(nums[mid] < target && target <= nums[RR]) {
                LL = mid + 1;
            } else {
                RR = mid - 1;
            }
            continue;
        }
        LL++;   // 保证越界
    }
    return -1;
}
```





### 09 贪心算法

**企图通过局部最优中获取全局最优的方法**

- 能举出反例的直接否定
- **不要纠结于贪心算法的证明**

贪心算法==往往和堆结构关联==



#### 1. 会议室宣讲时间安排

**题目描述**

- 一些项目要用同一间会议室宣讲
- 给定一个数组，包涵了所有项目的起止时间信息
- 要求合理安排时间，使得当天会议室进行的**宣讲次数最多**



**算法思路**

- 按照选项会结束时间排序，**最先结束的优先安排**，并删除数组中与该项目冲突的项目
- 重复上一步至数组为空



#### 2. 最小字典序 45

**题目描述**：给定一个字符串数组，要求合理安排拼接顺序，使得最终得到的**字典序最小**。

- 字典序直观定义：字典中排布的顺序（递增）
- 量化方法：将字符串转化为**27进制数进行比较** `a -> 1``z -> 26`
  - **同长字符串直接比较**
  - 较短的字符串**补0至同长**再比较



**算法思路** -- 自定义排序策略

- 将给定的字符串数组按自定义方法排序
- 安排排序后的顺序依次拼接即为**最小字典序**
- 注意自定义的比较策略**必须有传递性**

```c++
bool cmp(string& a, string& b) {
    return a + b < b + a;
}
```



#### 3. 最省钱的金条切法

**题目描述**

- 切割一根金条的费用数值为金条的长度
- 要求将一个L长的金条，切割成指定的小长度金条组
- 求最少花费的代价



**算法思路**

- 逆向思维，将arr合成L
- 通过堆结构贪心实现，**每次合成当前最短的两根金条**

本质是构建哈夫曼树



#### 4. 项目的最高利润IPO 502🌼

![image-20220812170245212](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812170245212.png)



**算法思路**

- **打怪升级贪心**，打能打得过的爆装备最好的怪
- `priority_queue<Project, vector<Project>, CapitalCmp> locks;`  当前未解锁的项目，按照门槛资本升序排列
- `priority_queue<Project, vector<Project>, ProfitCmp> unlocks;`  当前已解锁项目，按照收益降序排布



#### 5. 股票最大利润 63

![](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812170434985.png)



**朴素贪心**

从左到右遍历，维护当前位置前的**历史最低价**



#### 6. 两个非重叠子数组的最大和 1031

![image-20220814183619426](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814183619426.png)

**算法思路**

- 维护L窗口和M窗口
- L窗口和M窗口**同步右滑动**
- 记录L窗口的**历史最大值（贪心）**
- M窗口与L窗口历史最大值相加为**该M窗口能取得的最大和**
- 滑动过程取最优

```c++
int func(vector<int>& nums, int firstLen, int secondLen) {
    int idxF = 0, sumF = 0, maxSumF, idxS = firstLen, sumS = 0;
    int ans = 0;
    
    for(; idxF<firstLen; idxF++) {  // 初始化窗口L
        sumF += nums[idxF];
    }
    maxSumF = sumF; // 记录L窗口的最大值
    for(; idxS<firstLen+secondLen; idxS++) {    // 初始化窗口M
        sumS += nums[idxS];
    }
    ans = sumF + sumS;
    
    for(; idxS<nums.size(); idxF++, idxS++) {
        sumF = sumF - nums[idxF - firstLen] + nums[idxF];   // L窗口右滑
        sumS = sumS - nums[idxS - secondLen] + nums[idxS];  // M窗口右滑
        maxSumF = max(maxSumF, sumF);   // 贪心，取所有L窗口的最大值
        ans = max(ans, maxSumF + sumS);
    }
    
    return ans;
}
```



#### 7. 课程表Ⅲ 630

![image-20220814190252503](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814190252503.png)

**算法思路** -- 带反悔的贪心

- 按照结束时间升序排序，**优先修读结束时间早的课程**
- 当前课程不可修读时，**尝试挤掉已修读课程中耗时最长的课程**，以使当前总消耗时长最小

```c++
bool cmp(vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}

int scheduleCourse(vector<vector<int>>& courses) {
    ::sort(courses.begin(), courses.end(), cmp);    // 按照结束时间升序排序
    
    int costed = 0, cnt = 0;        // costed为已花费时间
    priority_queue<int> durations;  // 堆 用于反悔时贪心
    for(int ii=0; ii<courses.size(); ii++) {
        // [0]表示课时 [1]表示截止期限
        if(costed + courses[ii][0] <= courses[ii][1]) { // 当前课程可修读
            cnt++;
            costed += courses[ii][0];
            durations.push(courses[ii][0]);
        } else {
            // 当前课程不可修读时，尝试挤掉已修读课程中耗时最长的，否则直接pass当前课程
            if(!durations.empty() && courses[ii][0]<durations.top()) {
                costed = costed - durations.top() + courses[ii][0];
                durations.pop();
                durations.push(courses[ii][0]);
            }
        }
    }
    
    return cnt;
}
```







### 10 滑动窗口

#### 1. 滑动窗口最大值 239🌼

![image-20220812234311383](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812234311383.png)

**算法思路**

- 滑动窗口范围[L, R]
- 窗口中**存储下标值**
- R L **右滑过程中维护单调性**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211102124503575.png" alt="image-20211102124503575" style="zoom:50%;" />



```c++
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> ans;
    deque<int> helpDeque;	// 单调双端队列(性质同单调栈) 存储的是下标
    int L = 0, R = 0;		// 窗口范围[L, R]
    
    for(; R<nums.size(); R++){
        // 弹出已废前浪
        while(!helpDeque.empty() && nums[R] >= nums[helpDeque.back()]) {	// 维持deque中下标对应值的单调性
            helpDeque.pop_back();
        }
        helpDeque.push_back(R); // 当前位置入队
        
        if(R >= k-1){   		// 窗口成型后才能输出
            ans.push_back(nums[helpDeque.front()]);
            if(L >= helpDeque.front())  // L右滑可能导致队头过期
                helpDeque.pop_front();
            L++;
        }
    }
    
    return ans;
}
```



**注意事项**

- 双端队列实质上维护的是**会依次成为最大值的可能性**

  - 再无可能成为最大值的下标，**会被右侧更大值对应的下标淘汰**

- 时间复杂度**O(n)**

- 注意**值与值比**，**下标与下标比**



#### 2. 最长不含重复字符串的子字符串 48

![image-20220812231005412](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812231005412.png)



**算法思路**

- 维护滑动窗口[LL, RR]
- 保持win内不存在重复元素

```c++
int res = 0;
unordered_set<char> uset;						// 用于去重
for(int LL = 0, RR = 0; RR < s.size(); RR++) {	// 滑动窗口范围[LL, RR]
    while(uset.find(s[RR]) != uset.end()) { 	// 维持[LL, RR]范围内不存在重复元素
        uset.erase(s[LL]);
        LL++;
    }
    uset.insert(s[RR]);
    res = max(res, RR - LL + 1);
}
```





### 11 位运算

**异或运算性质**

- a ^ a = 0

- a ^ 0 = a

- a ^ b = b ^ a

- a ^ b ^ c = a ^ (b ^ c)

异或运算也可看做**2进制数的无进位相加**



#### 1. 只出现1次的数字 136

![image-20220613235011139](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220613235011139.png)

**全体异或**



#### 2.  数组中数字出现的次数 56-Ⅰ

![image-20220613234340858](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220613234340858.png)



**算法思路**

- 异或后，找到第1个非0位
- 以非0位分类，保证将只出现1次的两个数分到不同组当中
- 分组异或，找出各自组中只出现1次的数



#### 3.  数组中数字出现的次数 56-II

![image-20220613234636125](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220613234636125.png)

**解题思路**

- 遍历32位，累加得sum，%3及当前位取值



#### 4. 2 的幂

![image-20220812235858960](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812235858960.png)

**算法思路**

- n为2的幂，**要求n的二进制表示有且只有一个1**
- n & (n - 1)可以**清除n的最低位1**

```c++
bool isPowerOfTwo(int n) {
    if(n <= 0)			// 负数和0不肯能是2的幂
        return false;
    
    return (n & (n - 1)) == 0;
}
```



#### 5. 4的幂

![image-20220813000711751](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220813000711751.png)

**算法思路**

- 先满足**是2的幂**
- 进一步要求**唯一的1出现在特定位置上**：`0b0101 0101 0101 0101 0101 0101 0101 0101)`



#### 6. 使用位运算实现+ - * /

**题目描述**

- 给定int a b，要求不使用算数运算符，实现a b的 **+ - * /运算**

- 题目保证**运算结果不会越界**



**算法思路**

- **+ 运算**

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814220024816.png" alt="image-20220814220024816" style="zoom: 67%;" />

- **- 运算**
  - **b =（~b + 1）**得到 **-b**
  - 调用**+ 运算**：`add(a, b)`

- *** 运算**
  - **用移位** 模拟 **乘法算式过程**

- **/ 运算**
  - **符号位**（31）和**数值位**（0 ~ 30）分开考虑
  - **计算数值时**，a、b**都是正数**
  - **0 ~ 31数值位无非0、1**，用循环去**试**



**解题代码**

```c++
// +
int add(int a, int b) {
    int sum = a;
    int help = INT_MAX;					// 辅助抹除最高位，防 <<1 是的溢出（C++编译器特性）
    while(b != 0){   
        sum = (a ^ b);              	// 无进位相加结果
        b = (((a & b) & help) << 1);  	// b表示进位信息
        a = sum;
    }
    return sum;
}

// -
int minus(int a, int b){
    int b = (~b) + 1;
    return add(a, b);
}

// *
int multi(int a, int b){
    int ans = 0;
    while(b != 0){
        if((b & 1) != 0)
            ans = add(ans, a);
       	a = a << 1; 
        b = b >> 1;
    }
    
    return ans;
}

// /
int iNeg(int val){
    return (n < 0);
}

int divide(int a, int b){
    int x = (isNeg(a))  ? (~a + 1) : a;	// x = abs(a)
    int y = (isNeg(b))  ? (~b + 1) : b;	// y = abs(b)
    int ans = 0;
    for(int ii=31; ii>=0; ii--){
        if((x>>ii) >= y){			// 右移试探x是否足够大
            res |= 1 << ii;			// x足够大，当前位置1
            x = minus(x, y<<ii);	//
        }
    }
    
    if(isNeg(a) == isNeg(b))		// 同号得正
        return ans;
    return (~ans + 1);				// 异号得负
}

```





- 注意事项
  - **`a << 1`**前要将 a 的**最高位置0**，否则会溢出
  - **优先右移**试探，**防止溢出**





### 12 动态规划

依次解锁可用范围 + 额外限定条件，==使问题退化==

**建立状态转移关系**



#### 1. 最长递增子序列 300

![image-20220812205416657](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812205416657.png)



**算法思路**

```c++
int lengthOfLIS(vector<int>& nums) {
    vector<int> dp(nums.size());    // dp[i]表示nums[0-i]区间且pick nums[i]条件下的最长子序列长
    for(int ii=0; ii<nums.size(); ii++) {
        int ret = 0;
        for(int jj=0; jj<ii; jj++) {    // 遍历找最优
            if(nums[jj] < nums[ii])
                ret = max(ret, dp[jj]);
        }
        dp[ii] = ret + 1;
    }
    
    return dp.max();
}
```



#### 2. 打家劫舍

![image-20220812205942042](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812205942042.png)



**算法思路**

```c++
int rob(vector<int>& nums) {
    if(nums.size() == 1)    return nums[0];
    int len = nums.size();
    int dp[len];    // dp[i]表示抢nums[i]时的最优解
    dp[0] = nums[0], dp[1] = nums[1];
    
    for(int ii=2; ii<len; ii++) {
        dp[ii] = ::max(dp[ii-1], dp[ii-2] + nums[ii]);
    }
    return max(dp[len-1], dp[len-2]);
}
```



#### 3. 乘积最大子数组 152

![image-20220613222030616](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220613222030616.png)

**算法思路**

```c++
int maxProduct(vector<int>& nums) {
    int curMax = nums[0], curMin = nums[0], MAX = nums[0];
    for(int ii=1; ii<nums.size(); ii++) {		// 依次解锁可用范围
        int ret = curMax;
        curMax = max(nums[ii], max(curMax * nums[ii], curMin * nums[ii]));	// 必须pick nums[i]
        curMin = min(nums[ii], min(ret * nums[ii], curMin * nums[ii]));
        MAX = max(MAX, curMax);	// 全局最优
    }
    
    return MAX;
}
```



### 13 背包专题

#### 1. 01背包（每个物品只用一次） NC145

![image-20220812171308132](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812171308132.png)



**算法思路**

- dp[k]对应背包容量为k
- 依次解锁可用物品范围
- ==逆向更新==，避免重复使用

```c++
int knapsack(int V, int n, vector<vector<int> >& vw) {
    vector<int> dp(V+1);		// dp[k]对应背包容量为k
    for(int ii=0; ii<n; ii++) {	// 依次解锁可用物品范围
        for(int jj=V; jj>=vw[ii][0]; jj--) {	// 必须逆向更新⭐ 不能用到新的值 保证物品ii只被用到1次
            // 面对新解锁的物品ii 只有两种选择 1)用 2)不用
            // 1)对应dp[jj]
            // 2)对应dp[jj-vw[ii][0]] + vw[ii][1]
            dp[jj] = ::max(dp[jj], dp[jj-vw[ii][0]] + vw[ii][1]);
        }
    }
    
    return dp[V];
}
```



#### 2. 完全背包（物品可无限次使用） NC309

![image-20220812171308132](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812171308132.png)



**算法思路**

- dp[k]对应背包容量为k
- 依次解锁可用物品范围
- ==正向更新==，可以重复使用

```c++
int funcA(int v, int n, vector<vector<int> >& nums) {
    vector<int> dp(v+1);
    
    for(int ii=0; ii<n; ii++) {
        for(int jj=nums[ii][0]; jj<=v; jj++) {    // 正向更新⭐ 可以用到新的值 对应物品ii被使用多次
            dp[jj] = ::max(dp[jj], dp[jj-nums[ii][0]] + nums[ii][1]);
        }
    }
    
    return dp[v];
}
```



#### 3. 分割等和子集

![image-20220812174619918](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812174619918.png)

- 01背包变种
- **额外要求恰好装满**，所以要**初始标记不可达信息**



**算法思路**

- 将问题**转化为**能否用nums数组中的数，恰好凑出sum/2
- 依次解锁可用数字范围
- 初始标记为不可达
- ==逆向更新==，避免重复使用

```c++
vector<int> dp(sumV/2 + 1, -1);                     // -1表示当前位置不可达
dp[0] = 0; // base case
for(int ii=0; ii<nums.size(); ii++) {               // 依次解锁可用数字范围
    for(int jj=dp.size()-1; jj>=nums[ii]; jj--) {   // 只能用一次，所以从大到小更新
        if(dp[jj - nums[ii]] != -1) {               // 前一位置可达
            dp[jj] = 0;								// 标记当前位置可达
        }
    }
}
```



#### 4. 目标和

![image-20220812175414388](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812175414388.png)

- 01背包变种
- **额外要求恰好装满**，所以要初始标记不可达信息
- 要求**取法总数**



**算法思路**

- 将问题**转化为**能否用nums数组中的数，恰好凑出`(sum - target) / 2`
- 依次解锁可用数字范围
- 初始标记为不可达
- ==逆向更新==，避免重复使用

```c++
int T = (target + sum);
vector<int> dp(T+1, 0); // 0标记当前不可达
dp[0] = 1;				// base case
for(int ii=0; ii<nums.size(); ii++) {   // 依次解锁nums[ii]使用
    for(int jj=T; jj>=nums[ii]; jj--) {	// 逆向更行
        dp[jj] = dp[jj] + dp[jj - nums[ii]];
    }
}
```



#### 5. 零钱兑换

![image-20220812175856137](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812175856137.png)

- 完全背包变种
- 要求恰好装满



**算法思路**

- 依次解锁可用硬币范围
- 正向更新

```c++
vector<int> dp(amount + 1, -1); // 默认标记为不可达-1，非-1值表示当前最少用dp[i]种硬币凑成i
dp[0] = 0;  // base case

for(int ii=0; ii<coins.size(); ii++) {  		// 从左到右依次解锁可用硬币范围
    for(int jj=coins[ii]; jj<dp.size(); jj++) { // 尝试用新解锁的硬币更新dp表，由于硬币无限取用，从小到大更新
        if(dp[jj - coins[ii]] != -1) {  		// 前一位置不可达，无法触发更新
            if(dp[jj] == -1) { 
                dp[jj] = dp[jj - coins[ii]] + 1;
            } else {	// 最少种类
                dp[jj] = ::min(dp[jj], dp[jj - coins[ii]] + 1);
            }
        }
    }
}
```



### 14 股票专题

==依次解锁可用股票范围==



#### 1. 买卖股票的最佳时机 122

![image-20220812182014641](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812182014641.png)



**算法思路**

- 对于第i天，只有两种状态
  - **只有持有股票hold[i]**
  - **不持有股票unhold[i]**
- hold[i] & unhold[i]可由hold[i-1] & unhold[i]转移而来

```c++
vector<int> hold(prices.size());
vector<int> unhold(prices.size());
hold[0] = -prices[0];	// base case
unhold[0] = 0;
for(int ii=1; ii<prices.size(); ii++) {
    // 可以从hold[ii-1]不操作转化为hold[ii]状态
    // 也可以从unhold[ii-1]通过购入prices[ii]转化为hold[ii]状态
    hold[ii] = max(hold[ii-1], unhold[ii-1] - prices[ii]);
    unhold[ii] = max(unhold[ii-1], hold[ii-1] + prices[ii]);
}
```



#### 2. 买卖股票的最佳时机（限制2次交易） 123

![image-20220812182517313](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812182517313.png)



**算法思路**

- 对于第i天，只有4种状态（无事发生状态profit ≡ 0）
  - 买过1次
  - 完成1比交易
  - 完成1笔交易+买1次
  - 完成2比交易

```c++
vector<vector<int>> dp(N, vector<int>(4));
// [0]-买过1次 [1]-完成1比交易 [2]-完成1笔交易+买1次 [3]-完成2比交易
dp[0][0] = -prices[0];
dp[0][2] = -prices[0];
for(int ii=1; ii<N; ii++) {
    // 从本次不操作and本次操作中取最优
    dp[ii][0] = max(dp[ii-1][0], -prices[ii]);
    dp[ii][1] = max(dp[ii-1][1], dp[ii-1][0] + prices[ii]);
    dp[ii][2] = max(dp[ii-1][2], dp[ii-1][1] - prices[ii]);
    dp[ii][3] = max(dp[ii-1][3], dp[ii-1][2] + prices[ii]);
}
```



#### 3.  买卖股票的最佳时机（限制K次交易） 188

![image-20220812183354199](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812183354199.png)





#### 4. 最佳买卖股票时机（含冷冻期） 309

![image-20220812184953742](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220812184953742.png)

**算法思路**

- 对于第i天，只有3种状态
  - 当前持有股票
  - 当前不持有股票且处于冷冻期
  - 当前不持有股票且不处于冷冻期

```c++
int N = prices.size();
// 共3种状态
vector<int> hold(N);            // 1) 当前持有股票
vector<int> unhold_lock(N);     // 2) 当前不持有股票且处于冷冻期
vector<int> unhold_unlock(N);   // 3) 当前不持有股票且不处于冷冻期
hold[0] = -prices[0];           // base case

for(int ii=1; ii<N; ii++) {
    // 1) 可由 1) 3)转移
    hold[ii] = max(hold[ii-1], unhold_unlock[ii-1] - prices[ii]);
    // 2) 只能由 1) 转移
    unhold_lock[ii] = hold[ii-1] + prices[ii];
    // 3) 可由 2) 3) 转移
    unhold_unlock[ii] = max(unhold_lock[ii-1], unhold_unlock[ii-1]);
}
```





### 15  回溯

**backtracking = 递归嵌套for循环**

![image-20220821145724437](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220821145724437.png)

- for构成树的宽度
- 递归构成树的深度

- 回溯本质是穷举
- 回溯用于解决：组合问题、排列问题、子集问题

```c++
// 回溯框架
void backtracking(参数) {
	if (终⽌条件) {
		存放结果;
		return;
	}
    
	for (选择：本层集合中元素) {
		处理节点;
		backtracking(参数);	//递归
    	撤销处理结果;			 // 回溯
	}
}
```



注意==回溯过程中的剪枝和去重==



#### 1. 组合 77

![image-20220821152039467](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220821152039467.png)

**算法思路**

普通回溯

```c++
void backtracking(int pos) {
    if(pos == K) {
        arrs.push_back(arr);
        return ;
    }
    int ret = (pos == 0) ? 1 : arr[pos - 1] + 1;    // pos=0时从1开始枚举
    for(int val = ret; val <= N; val++) {
        arr.push_back(val);
        backtracking(pos+1);    // 递归
        arr.pop_back();         // 回溯
    }
}
vector<vector<int>> combine(int n, int k) {
    N = n, K = k;
    backtracking(0);
    return arrs;
}
vector<vector<int>> arrs;
vector<int> arr;
int N, K;
```



#### 2. 组合总和 II 40

![image-20220821155935657](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220821155935657.png)

**算法思路**

- 回溯

- 排序后==剪枝去重==

```c++
// 回溯 + 剪枝
// 只能从c[start, size())范围内pick
void backtracking(vector<int>& c, int start, int curSum) {  
    if(curSum == T) {
        arrs.push_back(arr);
        return ;
    }
    if(curSum > T || start >= c.size()) {
        return ;
    }
    for(int ii = start; ii < c.size(); ii++) { 	// 从start位置开始枚举
        if(ii > start && c[ii] == c[ii-1]) {	// 剪枝
            // c[ii] == c[ii-1]时pick c[ii]的分支是pick c[ii-1]分支的真子集
            continue;
        }
        arr.push_back(c[ii]);
        backtracking(c, ii + 1, curSum + c[ii]);    // 递归 不许重复使用所以i+1
        arr.pop_back();                             // 回溯
    }
}
vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    arrs.clear(), arr.clear();
    T = target;
    ::sort(candidates.begin(), candidates.end());   // 排序 去重准备
    backtracking(candidates, 0, 0);
    return arrs;
}
vector<vector<int>> arrs;
vector<int> arr;
int T;
```



### 16 并查集

![image-20220821165903443](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220821165903443.png)

- 初始状态下每个节点指向自身

- 集合状态下
  - 集合头指向自身
  - 其余节点直接/间接指向集合头
- 路径压缩后树的深度很浅，**但不能认为每个节点都直接挂载根节点下**
- 并查集操作时间复杂度为**O(α(N))**可以看作O(1)

```c++
class UnionFindSet{
public:
    // vector<int> nodes;
    vector<int> fathers;
    UnionFindSet(int n) {
       fathers.resize(n);
        for(int ii=0; ii<=0; ii++) {
            fathers[ii] = ii;	// 初始指向自身
        }
    }
    
    int find(int i) {
        if(fathers[i] == i) {	// 只有集合头指向自身
            return i;
        }
        fathers[i] = find(fathers[i]);	// 递归查找集合头 并进行路径压缩
        
        return fathers[i];
    }
    
    void unio(int i, int j) {	// 合并集合
        int i_fa = find(i);		// 找到i的祖先
        int j_fa = find(j);		// 找到j的祖先
        fathsers[j_fa] = i_fa;	// j的祖先指向i的祖先
    }
};
```



#### 1. 统计无向图中无法互相到达点对数 2316

![image-20220821175130993](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220821175130993.png)



**算法思路**

并查集

```c++
// find操作 过程中扁平化
int find(int i) {
    if(fathers[i] == i) {
        return i;
    }
    fathers[i] = find(fathers[i]);
    
    return fathers[i];
}
// 合并操作
void Union(int i, int j) {
    int fa_i = find(i);
    int fa_j = find(j);
    fathers[fa_j] = fa_i;   // 集合头j并入集合头i
}

long long countPairs(int n, vector<vector<int>>& edges) {
    fathers = vector<int>(n);
    for(int ii = 0; ii < n; ii++) { // 初始化 节点各自指向自身
        fathers[ii] = ii;
    }
    for(int ii = 0; ii < edges.size(); ii++) {
        // 根据边信息合并
        Union(edges[ii][0], edges[ii][1]);
    }
    unordered_map<int, long> umap;   // <集合头, 集合中元素数量>
    for(int ii = 0; ii < n; ii++) {
        umap[find(ii)]++;
    }
    long long cnt = 0;
    for(auto elem : umap) {
        cnt += elem.second * (n - elem.second);
    }
    return cnt >> 1;
}
vector<int> fathers;
```





### Kiwi

#### 01 N皇后 51

![image-20220814220412270](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814220412270.png)



**算法思路**

- 暴力尝试每一种可能，复杂度为**`O(n!)`**
- `vector<int> queens(n); ` 用queens[i]表示第i行皇后所处列
  - ⭐本题灵魂，如此自然保证行不冲突，且形式简洁⭐
- 注意冲突检测形式

```c++
bool check(int row, int col) {  // 检查冲突
    for(int ii=0; ii<row; ii++) {
        // 列冲突或斜角冲突 |\/
        if(queens[ii]==col || queens[ii]-ii==col-row || queens[ii]+ii==col+row) {    
            return false;
        }
    }
    return true;
}
void process(int row) { // 当前要在row行安置皇后
    if(row == N) {
        CNT++;
        return ;
    }
    for(int jj=0; jj<N; jj++) { // 遍历尝试将row行的皇后放置在jj位置
        if(check(row, jj)) {
            queens[row] = jj;   // 放在第jj列
            process(row + 1);
        }
    }
}
int totalNQueens(int n) {
    N = n, CNT = 0;
    queens = vector<int>(N);
    process(0);
    
    return CNT;
}
int N;              // 行数
int CNT;            // 解数
vector<int> queens; // queens[i]表示第i行皇后所处位置⭐本题灵魂，如此自然保证行不冲突，且形式简洁⭐
```



#### 02 二维数组中的查找 04

![image-20220814222401207](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814222401207.png)



**算法思路**：**从右上角出发进行查找**

- cur < target：**向下走**

- cur > target：**向左走**

- cur == target：返回


<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20211125101117081.png" alt="image-20211125101117081" style="zoom:50%;" />





#### 03 求1+2+...+n 64

![image-20220814222938876](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814222938876.png)



算法思路

- 递归 + **逻辑运算符短路性质**


```c++
int sumNums(int n) {
    (n == 1) || (n = n+sumNums(n-1));   // 递归 + 逻辑运算符短路
    return n; 
}
```

- **天秀**

```c++
int sumNums(int n) {
    char chs[n][n+1];
    return sizeof(chs) >> 1; 
}
```



#### 04 数组中出现次数超过一半的数字 39

![image-20220814223158573](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814223158573.png)

**投票算法**



#### 05 正则表达式匹配 10

![image-20220814232031187](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220814232031187.png)



**算法思路 -- 有限状态机递归**

- **p[jj+1] 不为 ***
  - 当前位置匹配，s p 各自后移1位
  - 当前位置不匹配，直接返回false
- **p[jj+1] 为 ***
  - 当前位置匹配（可以**向3种状态转移**）
    - **s后移1位 p后移2位**
    - **s不动 p后移2位**
    - **s后移1位 p不动**
  - 当前位置不匹配
    - **s不动 p后移2位**

关键在于**将_* 两两绑定**



#### 06 可怜的小猪 458

![image-20220622230642552](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220622230642552.png)

**算法思路**

- 信息论知识
  - 数学yyds
  - 别去想具体执行的方式
- **1 只猪，经过 k 轮测试，有 k+1 种可能状态**
  - 第 1 轮死
  - 第 2 轮死
  - …… 
  - 第 k 轮死
  - k 轮结束后依然活着
- 所以 n 只猪， 经过 k 轮测试 ， **有 (k+1)^n 种状态，也就能检测同等数量的水**
  - 每1种状态映射到1桶水即可

```c++
int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
    int round = minutesToTest / minutesToDie;       // 轮数
    int n = ceil(log(buckets) / log(round+1)) ;     // 计算公式
    
    return n;
}
```



#### 07 质数因子

![image-20220329210031189](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220329210031189.png)



**解题思路**

遍历[2, sqrt(n)]，如果能整除，直接输出

- **一定不会输出非质数因数**（==非质数因数会被提前分解==）
- sqrt(n)范围由数学保证
- 需要收尾处理（n不能分解的很彻底时）
- 时间复杂度O(n^1/2^)

**避免踩直接判断质数的坑**

```c++
vector<int> getOddFactors(int n) {
    int odd = 2;
    vector<int> odds;
    while(odd <= sqrt(n)) {
        if(n % odd) {
            odds.push_back(odd);
            n /= odd;
        } else {
        	odd++;   
        }
    }
    if(n > 1)	// 收尾处理
        odds.push_back(n);
    
    return odds;
}

// ps 判断质数
// bool isPrime(n) {
//     if(n <= 1) {
//         return false;
//     }
//     for(int i = 2; i * i <= n; i++) {
//         if(n % i == 0)
//          	return false;
//     }
//     
//     return true;
// }
```



#### 08 计数质数

![image-20220820230353363](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220820230353363.png)

**算法思路**

埃氏筛

**避免踩直接判断质数的坑**

```c++
int countPrimes(int n) {
    vector<int> isOdd(n, 1);  // 默认全质数
    int cnt = 0;
    for(int i = 2; i < n; i++) {
        if(isOdd[i] == 1) {
            cnt++;
            // 此时i为新找到的质数 将所有i的倍数标注为合数 2i 3i 4i ...
            // 更优化的 从i*i开始查找
            for(long k = i; k * i < n; k++) {
                isOdd[k * i] = 0;
            }
        }
    }
    return cnt;
}
```







### Coding心法

#### 1 条件反射

|    题干输入    |         大脑反射输出         |                补充说明                |
| :------------: | :--------------------------: | :------------------------------------: |
|  ==有序数组==  | 二分查找<br />双指针顺序夹逼 |               34<br />57               |
|    ==逆序==    |            栈结构            |                                        |
|    ==DFS==     |            栈结构            |          树、图的深度优先遍历          |
|    ==BFS==     |             队列             |          树、图的广度优先遍历          |
|    ==贪心==    |            堆结构            |                                        |
|   ==大数据==   |           哈希分流           |      哈希函数能做到**按种类均分**      |
| ==二叉搜索树== |         中序序列递增         |                                        |
|  ==二维数组==  |            图结构            | vector<vector\<int>>**提供图的边信息** |
|                |                              |                                        |
|                |                              |                                        |



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



#### 25 图的表示

用vector存储图是一种简便表示，**将编号为i的图节点存储在vec[i]位置**

省去了edge指针在C++中内存管理不变的困扰

```c++
// LC 851
class Node {
public:
    int num;
    int quiet;
    vector<int> next;
};

// 建图
vector<Node> graph(N);
for(int ii=0; ii<N; ii++) {
    graph[ii].num = ii;
    graph[ii].quiet = quiet[ii];
}
for(auto& vec : richer) {
    graph[vec[1]].next.push_back(vec[0]);
}
```





### 剑指Offer第二版

> 20211120 - 20211221

#### 提高代码质量

- 规范性
- 完整性
- 鲁棒性

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220419225013831.png" alt="image-20220419225013831" style="zoom: 80%;" />



#### 解决复杂问题

- 画图：图解
- 举例：模拟归纳
- 分解：各个击破

![image-20220419225237182](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220419225237182.png)





#### 面试综合能力

- 沟通能力：明确输入输出和题目要求
- 学习能力：快速接受新概念
- 知识迁移能力：从简单题中得到提示，从而解决复杂题目

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220419225422322.png" alt="image-20220419225422322" style="zoom:50%;" />



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

