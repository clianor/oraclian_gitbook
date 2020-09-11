# Tree

트리란 나무를 뒤집은 모습과 같다고하여 트리라는 이름이 붙은 자료구조로서 컴퓨터의 폴더 구조와 계층적으로 표현되는 조직도와 같은 것들이 트리 구조의 대표적인 예로 사용이 됩니다.

자료구조에서 트리란 1개 이상의 노드를 가지는 집합으로서 루트 노드로부터 뻗어나간 가지와 같이 또 다른 트리를 가지는 구조가 될 수 있으며 이를 서브트리라고 합니다.

아래는 트리 구조를 설명하는 네이버 지식백과의 사진입니다.

![&#xCD9C;&#xCC98;: https://terms.naver.com/entry.nhn?docId=2270428&amp;cid=51173&amp;categoryId=51173](../../../.gitbook/assets/image%20%287%29.png)

트리를 javascript로 구현하면 아래와 같이 구현할 수 있습니다.  
다음은 이진트리입니다.

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}

const _insert = (root, node) => {
  if (root === null) {
    return node;
  } else if (node.data < root.data) {
    root.left = _insert(root.left, node);
    return root;
  } else if (node.data > root.data) {
    root.right = _insert(root.right, node);
    return root;
  }

  return root;
};

class Tree {
  constructor(data) {
    if (data) {
      this.root = new Node(data);
    } else {
      this.root = null;
    }
  }

  add(data) {
    const node = new Node(data);

    if (this.root === null) {
      this.root = node;
      return;
    }

    if (node.data < this.root.data) {
      this.root.left = _insert(this.root.left, node);
    } else {
      this.root.right = _insert(this.root.right, node);
    }
  }
}

const tree = new Tree(5);
tree.add(3);
tree.add(7);
tree.add(1);
tree.add(4);
tree.add(2);
tree.add(9);
console.log(tree);

```

위의 내용을 그림으로 표현하면 아래의 그림과 같이 됩니다.

![](../../../.gitbook/assets/image%20%2812%29.png)



