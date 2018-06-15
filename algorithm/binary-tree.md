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
``` C++
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
        while( p->ltag == Link ){
            p = p->lchild;
        }
        visit(p);
        while( p->rtag == Thread && p->rchild != T){
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



