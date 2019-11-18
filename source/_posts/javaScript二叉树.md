---
title: javaScript二叉树
date: 2019-11-18 21:43:14
tags: 
- 面试 
- 数据结构
- 算法
---
## 二叉树结构

## 中序遍历
> 从二叉树的根结点出发，当第二次到达结点时就输出结点数据，按照先向左在向右的方向访问。
左子树--根节点--右子树
## 前序遍历
> 从二叉树的根结点出发，当第一次到达结点时就输出结点数据，按照先向左在向右的方向访问。
根节点--左子树--右子树
## 后序遍历
> 从二叉树的根结点出发，当第三次到达结点时就输出结点数据，按照先向左在向右的方向访问。
左子树--右子树--根节点
## 层级遍历
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
    getMax(){
        let maxNode = function(node){
            return node.right? maxNode(node.right) : node.data
        }
        return maxNode(this.root)
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