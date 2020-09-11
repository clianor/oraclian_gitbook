# BFS \(너비 우선 탐색\)

너비 우선 탐색은 깊이 우선 탐색과는 다르게 루트 노드로부터 시작해서 인접한 노드들을 하나씩 찾아가는 방법입니다.  
depth 0의 모든 노드를 탐색한 뒤 depth 1의 모든 노드를 탐색하고 depth 2를 탐색하는 식으로 동작합니다.

너비 우선 탐색은 더이상 탐색할 depth가 없을때까지 탐색하면 되기에 아래와 같이 구현할 수 있습니다.

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

const _BFS = (nodes, arr) => {
  if (nodes.length === 0) return arr;

  const temp = [];
  for (node of nodes) {
    arr.push(node.data);

    node.left && temp.push(node.left);
    node.right && temp.push(node.right);
  }

  arr = _BFS(temp, arr);

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

  BFS() {
    return _BFS(this.root ? [this.root] : [], []);
  }
}

const tree = new Tree(5);
tree.add(3);
tree.add(7);
tree.add(1);
tree.add(4);
tree.add(2);
tree.add(9);
console.log(tree.BFS());
// [ 5, 3, 7, 1, 4, 9, 2 ]
```

