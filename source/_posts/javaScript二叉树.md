---
title: javaScript二叉树
date: 2019-11-18 21:43:14
tags: 
- 面试 
- 数据结构
- 算法
---
## 递归遍历二叉树
#### 中序遍历
> 从二叉树的根结点出发，当第二次到达结点时就输出结点数据，按照先向左在向右的方向访问。
左子树--根节点--右子树
#### 前序遍历
> 从二叉树的根结点出发，当第一次到达结点时就输出结点数据，按照先向左在向右的方向访问。
根节点--左子树--右子树
#### 后序遍历
> 从二叉树的根结点出发，当第三次到达结点时就输出结点数据，按照先向左在向右的方向访问。
左子树--右子树--根节点
```javaScript
class treeNode{
    constructor(data=null){
        this.data = data
        this.left = null
        this.right = null
        return this
    }
}
class Tree{
    constructor(){
        this.root = null
    }
    //创建二叉树
    insertArr(arr){
        let that = this
        let insert = function(node,newNode){
            if(newNode.data < node.data){
                if(node.left === null){
                    node.left = newNode
                }else{
                    insert(node.left,newNode)
                }
            }else{
                if(node.right === null){
                    node.right = newNode
                }else{
                    insert(node.right,newNode)
                }
            }
        }
        arr.forEach((a)=>{
            let newNode = new treeNode(a)
            if(that.root === null){
                that.root = newNode
            }else{
                insert(that.root,newNode)
            }
        })
        return that.root
    }
    //中序遍历
    inOrder(){
        let arr = []
        let inOrderNode = function(node){
            if(node){
                inOrderNode(node.left)
                arr.push(node.data)
                inOrderNode(node.right)
            }
        }
        inOrderNode(this.root)
        return arr
    }
    //前序遍历
    preOrder(){
        let arr = []
        let preOrderNode = function(node){
            if(node){
                arr.push(node.data)
                preOrderNode(node.left)
                preOrderNode(node.right)
            }
        } 
        preOrderNode(this.root)
        return arr
    }
    //后序遍历
    postOrder(){
        let arr = []
        let postOrder = function(node){
            if(node){
                postOrder(node.left)
                postOrder(node.right)
                arr.push(node.data)
            }
        }
        postOrder(this.root)
        return arr
    }
    //层序遍历
    serialize(){
        let serializeNode = function(root){
            if (root === null) return [];
            let res = [];
            let node = root,
                queue = [node];
            while (queue.length) {
                node = queue.shift();
                if (node === null) {
                    res.push(null);
                } else {
                    res.push(node.data);
                    node.left? queue.push(node.left):'';
                    node.right? queue.push(node.right):'';
                }
            }
            return res;
        }
        return serializeNode(this.root)
    }
}

let tree = new Tree()
tree.insertArr([6,3,2,5,8,7,9])
console.log(tree.inOrder())     //[ 2, 3, 5, 6, 7, 8, 9 ]中序遍历
console.log(tree.preOrder())    //[ 6, 3, 2, 5, 8, 7, 9 ]前序遍历
console.log(tree.postOrder())   //[ 2, 5, 3, 7, 9, 8, 6 ]后序遍历
console.log(tree.serialize())   //[ 6, 3, 8, 2, 5, 7, 9 ]层序遍历

```
## 非递归遍历二叉树
#### 前序遍历
> 我们用栈arr来保存遍历过程中的节点
首先将根节点保存到栈中，循环遍历栈直到栈为空
因为是前序遍历，因此第一步打印根节点
打印完毕后，查询当前节点是否有右孩子，右孩子要先入栈
查询当前节点是否有左孩子，左孩子后入栈
右孩子比左孩子先入栈的原因是，在下一个循环中左孩子要先出栈，右孩子后出栈
右孩子出栈后又相当于一个根节点，先打印根节点，然后再继续向下执行上述类似查找步骤
#### 中序遍历
> 非递归中序遍历首先要访问左孩子，然后在访问根节点，最后在访问右孩子
因此，在执行过程中，需要有一次跨域根节点的过程，此时栈已经空了，而这个时候是需要访问右孩子的
所以，当栈为空，且节点为空时表明整个访问过程结束
否则，如果当前节点为空，表明已经到达最底部；如果当前节点不为空，继续向下查找
#### 后序遍历
> 我们采用栈来保存之前经过的节点，首先将根节点保存到栈中，并用temp记录下栈顶的节点
我们要使用temp来判断是否要继续向下搜寻，直到temp的左孩子和右孩子都为空。
我们需要使用一个循环来遍历栈arr，直到arr为空
第一种情况就是不断向下搜寻的过程，最近一次打印的节点traverseNode既不是temp的左孩子，也不是temp的右孩子
第二种情况也是不断的搜寻过程，只不过此时刚打印完temp的左孩子traverseNode，转到了temp的右孩子
第三种情况就是temp的左孩子和右孩子都不存在，说明此时已经到了最底端，需要从弹出栈顶的元素并进行打印，同时更新最近打印过的节点traverseNode

```javaScript
class treeNode{
    constructor(data=null){
        this.data = data
        this.left = null
        this.right = null
        return this
    }
}
class Tree{
    constructor(){
        this.root = null
    }
    //创建二叉树
    insertArr(arr){
        let that = this
        let insert = function(node,newNode){
            if(newNode.data < node.data){
                if(node.left === null){
                    node.left = newNode
                }else{
                    insert(node.left,newNode)
                }
            }else{
                if(node.right === null){
                    node.right = newNode
                }else{
                    insert(node.right,newNode)
                }
            }
        }
        arr.forEach((a)=>{
            let newNode = new treeNode(a)
            if(that.root === null){
                that.root = newNode
            }else{
                insert(that.root,newNode)
            }
        })
        return that.root
    }
    //中序遍历
    inOrder(){
        let node = this.root
        let arr = []
        let stack = []
        if(!node) return arr
        while(stack.length||node){
            if(node){
                stack.push(node)
                node = node.left
            }else{
                node = stack.pop()
                arr.push(node.data)
                node = node.right
            }
        }
        return arr
    }
    //前序遍历
    preOrder(){
        let node = this.root
        let arr = []
        let stack = []
        if(!node) return arr
        stack.push(node)
        while(stack.length){
            let node = stack.pop()
            arr.push(node.data)
            if(node.right) stack.push(node.right)
            if(node.left) stack.push(node.left)
        }
        return arr
    }
    //后序遍历
    postOrder(){
        let node = this.root
        let arr = []
        let stack = []
        let pos = null
        let temp = null
        if(!node) return arr
        stack.push(node)
        while(stack.length){
            temp = stack[stack.length - 1]
            if(temp.left && temp.left !== pos && temp.right !== pos){
                stack.push(temp.left)
            }else if(temp.right && temp.right !== pos){
                stack.push(temp.right)
            }else{
                pos = stack.pop()
                arr.push(pos.data)
            }
        }
        return arr
    }
}

let tree = new Tree()
tree.insertArr([ 6, 3, 2, 5, 8, 7, 9])
console.log(tree.inOrder())     //[ 2, 3, 5, 6, 7, 8, 9 ]中序遍历
console.log(tree.preOrder())    //[ 6, 3, 2, 5, 8, 7, 9 ]前序遍历
console.log(tree.postOrder())   //[ 2, 5, 3, 7, 9, 8, 6 ]后序遍历
```