# DFS \(깊이 우선 탐색\)

깊이 우선 탐색은 한 방향으로 계속 가다가 더 이상 갈 곳이 없으면 가장 가까운 갈림길로 돌아와 그곳으로부터 다시 탐색하는 방법입니다.

![&#xCD9C;&#xCC98;: https://ko.wikipedia.org/wiki/%EA%B9%8A%EC%9D%B4\_%EC%9A%B0%EC%84%A0\_%ED%83%90%EC%83%89](../../../.gitbook/assets/image%20%288%29.png)

이는 재귀를 사용하면 쉽게 구현이 가능하고 아래와 같이 구현할 수 있습니다.

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

const _DFS = (root, arr) => {
  if (root === null) return arr;
  arr.push(root.data);
  arr = _DFS(root.left, arr);
  arr = _DFS(root.right, arr);
  return arr;
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

  DFS() {
    return _DFS(this.root, []);
  }
}

const tree = new Tree(5);
tree.add(3);
tree.add(7);
tree.add(1);
tree.add(4);
tree.add(2);
tree.add(9);
console.log(tree.DFS());
// result: [ 5, 3, 1, 2, 4, 7, 9]
```

![](../../../.gitbook/assets/image%20%2810%29.png)



