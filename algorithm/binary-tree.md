# 二叉树

## 二叉树的性质


## 二叉树的存储结构
- 顺序存储
    > 按层序遍历的次序依次存储在数组中
    ``` C++
    #define MAX_TREE_SIZE 100
    typedef ElemType SqBiTree[MAX_TREE_SIZE];
    ```
- 链式存储
    ``` C++
    typedef struct _BiTNode* BiTree;
    typedef struct _BiTNode{
        ElemType    data;
        struct BiTree lchild, rchild; 
        bool visited;
    }BiTNode;
    ```

## 二叉树的遍历
- 先序遍历
    访问根节点->先序遍历左子树->先序遍历右子树
- 中序遍历
    中序遍历左子树->访问根节点->中序遍历右子树
- 后序遍历
    后序遍历左子树->后序遍历右子树->访问根节点


### 递归遍历

先序遍历
``` C++
void PreOrderTraverse(BiTree T){
    if(T){
        visit(T->data);
        preOrderTraverse(T->lchild);
        preOrderTraverse(T->rchild);
    }
}
```

中序遍历
``` C++
void InOrderTraverse(BiTree T){
    if(T){
        inOrderTraverse(T->lchild);
        visit(T->data);
        inOrderTraverse(T->rchild);
    }
}
```

后序遍历
``` C++
void PostOrderTraverse(BiTree T){
    if(T){
        postOrderTraverse(T->lchild);
        postOrderTraverse(T->rchild);
        visit(T->data);
    }
}
```


### 非递归遍历
先序遍历
``` C++
void PreOrderTraverse(BiTree T){
    stack<BiTree> stack;
    BiTree p = T;
    while(p || !stack.empty()){
        if(p){
            visit(p);
            stack.push(p);
            p = p->lchild;
        }else{
            p = stack.pop();
            p = p->rchild;
        }
    }
}
```

中序遍历
``` C++
void InOrderTraverse(BiTree T){
    stack<BiTree> stack;
    BiTree p = T;
    while(p || !stack.empty()){
        if(p){
            stack.push(p);
            p = p->lchild;
        }else{
            p = stack.pop();
            visit(p);
            p = p->rchild;
        }
    }
}
```

后序遍历
``` C++
void PostOrderTraverse(BiTree T){
    stack<BiTree> stack;
    BiTree p = T;
    while(p || !stack.empty()){
        if(p){
            stack.push(p);
            p = p->lchild;
        }else{
            p = stack.top();
            if(p->rchild == NULL || p->rchild->visted){
                visit(p);
                p->visted = true;
                stack.pop();
                p = NULL;
            }else{
                p = p->rchild;
            }
        }
    }
}
```

## 创建二叉树

按照先序遍历读取ABC^^DE^G^^F^^^
```C++
void CreateBiTree(BiTree &T){
    ElemType data;
    cin >> data;
    if(data == NULL){
        T = NULL;
    }else{
        if(!(T = (BiTree)malloc(sizeof(BiTNode))){
            exit(OVERFLOW);
        }
        T->data = data;
        createBiTree(T->lchild);
        createBiTree(T->rchild);
    }
}
```



## 线索二叉树

### 线索二叉树的定义
若节点有左子树，则其lchild域指示其左子树，否则令lchild域指示其前驱；若节点有右子树，则其rchild域指示其右子树，否则令rchild域指示其后继。还需要增加两个标志域，ltag和rtag，其中ltag==0，lchild域指示节点的左子树；ltag==1，lchild指示节点的右子树。

### 线索二叉树的存储表示
```C++
typedef enum PointerTag { Link, Thread}; // Link == 0:指针  Thread==1:线索
typedef struct _BiThrNode *BiThrTree
typedef struct _BiThrNode{
    ElemType data;
    BiThrTree lchild, rchild;
    PointerTag ltag,rtag;       
}BiThrNode;
```


### 线索二叉树遍历

线索二叉树中序遍历
```C++
// 其中T是头结点，其lchild域指向二叉树的根节点，rchild指向中序遍历的最后一个节点
void InOrderTraverseThr(BiThrTree T){
    BiThrTree p = T->lchild;
    while(p != T){
        while( p->ltag == Link ){       // 访问左子树
            p = p->lchild;
        }
        visit(p);                       // 访问根节点
        while( p->rtag == Thread && p->rchild != T){    // 访问右子树
            p = p->rchild;
            visit(p);
        }
        p = p->rchild;
    }
}
```

线索二叉树先序遍历
```C++
void PreOrderTraverseThr(BiThrTree T){
    BiThrTree p = T->lchild;
    while(p != T){
        while( visit(p) && p->ltag == Link ){
            p = p->lchild;
        }
        while( p-rtag == Thread && p-rchild != T){
            p = p->rchild;
            visit(p);
        }
        p = p->rchild;
    }
}
```

### 二叉树线索华
二叉树中序线索化
```C++
// 中序遍历二叉树，并将其中序线索华，head指向头结点，
// 头结点的lchild指向二叉树的根节点，rchild指向中序遍历时访问的最后一个节点
void InOrderThreading(BiThrTree & head, BiThrTree T){
    if(!(head = (BiThrTree) malloc(sizeof(BiThrNode)))){ // 建立头结点
        exit(OVERFLOW);
    }
    head->ltag = Link;
    head->rtag = Thread;
    head->rchild = Thrt;
    if(!T){
        head->lchild = head;
    }else{
        head->lchild = T;
        BiThrTree pre = head;
        InThreading(T, pre);
        pre->rchild = head;
        pre->rtag = Thread;
        head->rchild = pre;
    }
}

void InThreading(BiThrTree p, BiThrTree & pre){
    if(p){
        InThreading(p->lchild, pre);
        if( ! p->lchild){
            p->ltag = Thread;
            p->lchild = pre;
        }
        if(!pre->rchild){
            pre->rtag = Thread;
            pre->rchild = p;
        }
        pre = p;
        InThreading(p->rchild, pre);
    }
}
```



# 树和森林

## 树的存储结构
- 双亲表示法
    以一组连续空间存储树的结构，同事每个节点附设一个指示器指示其双亲节点在链表中的位置。

    ```C++
    #define MAX_TREE_SIZE 100
    typedef struct PTNode {
        ElemType  data;
        int       parent; //双亲位置
    } PTNode;

    typedef struct PTree{
        PTNode nodes[MAX_TREE_SIZE];
        int      r;     // 根的位置
        int      n;     // 节点数
    }PTree
    ```
- 孩子表示法
    把每个节点的孩子节点排列起来，看成是一个线性表
    ```C++
    typedef struct _CTNode *CTNode;
    struct _CTNode {                // 孩子节点
        int         child;
        CTNode      next;
    }

    typedef struct{
        ElemType    data;
        CTNode    firstchild;       //孩子链表头指针
    }CTBox;

    typedef struct{
        CTBox   nodes[MAX_TREE_SIZE];
        int     n;                  // 节点数
        int     r;                  // 根的位置
    }CTree
    ```

- 孩子兄弟表示法
    以二叉链表作为树的存储结构，链表中节点的两个链域分别指向该节点的第一个孩子节点和下一个兄弟节点。
    ```C++
    typedef struct _CSNode *CSNode;
    typedef struct _CSNode {
        ElemType    data;
        CSNode      firstchild;     // 第一个孩子
        CSNode      nextsibling;    // 下一个兄弟
    }CSNode
    ```

## 森林和二叉树的转换
任何一个棵树对应的二叉树的右子树必是空的，将森林中的第二棵树的根节点看成是第一棵树的根节点的兄弟，则可以导出森林和二叉树的对应关系。


## 树和森林的遍历
1. 先序遍历森林
2. 中序遍历恩林
3. 后序遍历森林

当以二叉链表作为树的存储结构时，树的先序遍历和中序遍历与二叉树的先序遍历和中序遍历相同。




## 赫夫曼树(最优二叉树)

### 赫夫曼树定义
节点的带权路径长度为从该节点到树根之间的路径长度与节点上权的乘积。

树的带权路径长度为树中所有叶子节点的带权路径长度之和,记$WPL=\sum_{k-1}^n w_kl_k$。其中WPL最小的二叉树成为最优二叉树。

### 赫夫曼树构造
1. 给定的n个权值$\{w_1,w_2,...,w_n\}$构成n棵二叉树的集合$F=\{T_1,T_2,T_3\}$。
2. 在F中选取两棵根节点的权值最小的树作为左右子树构成一棵新的二叉树，且置信的二叉树的根节点的权值为其左、右子树上根节点的权值之和。
3. 在F中删除这两棵树，同时将得到的二叉树加入F中
4. 重复$(2)$和$(3)$，知道F中包含一棵树为止，这棵树便是赫夫曼树。


### 赫夫曼编码
*前缀编码*：任何一个字符的编码都不是另一个字符的编码的前缀，这种编码成为前缀编码。

约定二叉树的左分支，表示字符'0'，右分支表示字符'1'，则把从根节点到叶子节点的路径上分支字符组成的字符串作为该叶子节点字符的编码。
如此得到的必为二进制前缀编码。

![赫夫曼编码示意图](./images/huffman_code.png)

## 二叉排序树

### 二叉排序树定义
1. 若它的左子树不为空，则它的左子树的所有节点的值均小于它的根节点
2. 若它的右子树不为空，则它的右子树的所有节点的值均大于它的根节点
3. 它的左、右子树也分别为二叉排序树


### 二叉排序树查找
```C++
BiTree SearchBST(BiTree T, KeyType key){
    if( (!T) || Equals(key, T->data.key)){
        return T;
    }else if(LessThan(key, T->data.key)){
        retrun SearchBST(T->lchild, key);
    }else{
        return SearchBST(T->rchild, key);
    }
}
```

### 二叉排序树插入
```C++
#define TRUE 1
#define FALSE 0
typedef int Status;

Status SearchBST(BiTree T, KeyType key, BiTree f, BiTree &p){
    // 在根指针T所指的二叉排序树中递归地查找其关键字等于key的数据元素，若查找成功，
    // 则指针p指向该数据元素节点，并返回TRUE，否则指针p指向查找路径上访问的
    // 最后一个节点并返回FALSE，指针f指向T的双亲，其初始调用值为NULL
    if(!T){
        p = f;
        return FALSE;
    }else if( Equals(key, T->data.key)){
        p = T;
        return TRUE;
    }else if( LessThan(key, T->data.key)){
        return SearchBST(T->lchild, key, T, p);
    }else {
        return SearchBST(T->rchild, key, T, p);
    }
} // SearchBST

Status InsertBST(BiTree &T, ElemType e){
    // 当二叉排序树T中不存在关键则等于e.key的数据元素时，
    // 插入e并返回TRUE，否则返回FALSE
    if(!SearchBST( T, e, NULL, p)){
        s = (BiTree) malloc(sizeof(BiTNode));
        s->data = e;
        s->lchild = s->rchild = NULL;
        if(!p){
            T = s;
        }else if( LessThan(e.key, p->data.key)){
            p->lchild = s;
        }else{
            p->rchild = s;
        }
        return TRUE;
    }else{
        return FALSE;
    }
} // InsertBST
、、、

### 二叉排序树删除
```C++
Status DeleteBST( BiTree &T, KeyType key){
    if(!T){
        return FALSE;
    }else{
        if(EQ(key, T->data.key)){
            return Delete(T);
        }else if(LT(key, T->data.key)){
            return DeleteBST(T->lchild, key);
        }else {
            return DeleteBST(T->rchild, key);
        }
    }
}

// 设需要删除的节点为p
// 当找出p的左子树和右子树均不为空时，找出p的左子树的最大值s，
// 将s的值替换p的值，删除s点
Status Delete(BiTree &p){
    BiTree q;
    if(!p->rchild){             // 只需重接左子树
        q = p; p = p->lchild; free(q);
    }else if(!p->lchild){       // 只需要重接右子树
        q = p; p = p->rchild; free(q);
    }else{      // 左右子树都不为空
        q = p; s = p->lchild;
        while(s->rchild){
            q = s; s = s->rchild;
        }
        p->data = s->data;
        if(q != p){
            q->rchild = s->lchild;
        }else{
            q->lchild = s->lchild;
        }
        free(s);
    }
    return TRUE;
} // Delete
```


