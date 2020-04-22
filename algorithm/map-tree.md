## 遍历二叉树 {docsify-ignore}

![tree](./img/tree.png)

### 深度遍历(DFS)

#### 递归方式实现

```js
// 递归遍历
function mapTree(node, cb) {
  if (!node) {
    return;
  }
  cb(node); // 先序遍历
  mapTree(node.left, cb);
  // cb(node); // 中序遍历
  mapTree(node.right, cb);
  // cb(node); // 后序遍历
}
```

#### 非递归方式实现

##### 先序遍历

```jsx
/*react*/
<desc>
  Hello `world`
  * a
</desc>
<script>
  export default class Application extends React.Component {
    constructor(props) {
      super(props)
      this.state = {
        res: []
      };
      this.node = {
        val: 'A',
        left: {
          val: 'B',
          left: {
            val: 'D',
            right: {
              val: 'G'
            }
          }
        },
        right: {
          val: 'C',
          left: {
            val: 'E'
          },
          right: {
            val: 'F'
          }
        }
      }
    }

    run = () => {
      this.setState({res: []})
      this.mapTree(this.node, (n) => {
        this.setState((prevState) => ({
          res: [...prevState.res, n.val]
        }))
      })
    }

    mapTree = (node, cb) => {
      if (!node) {
        return;
      }
      const s = [node];
      while(s.length) {
        const n = s.pop();
        cb(n);
        if (n.right) s.push(n.right);
        if (n.left) s.push(n.left);
      }
    }

    render() {
      const { res } = this.state;
      return (
        <div>
          <div>{res.join(', ')}</div>
          <button onClick={this.run}>试一下</button>
        </div>
      )
    }
  }
</script>
```

```js
// 非递归遍历
function mapTree2(node, cb) {
  if (!node) {
    return;
  }
  const s = [node];
  while (s.length) {
    const n = s.pop();
    cb(n);
    if (n.right) s.push(n.right);
    if (n.left) s.push(n.left);
  }
}
```

##### 中序遍历

```js
// 非递归遍历
function mapTree2(node, cb) {
  const s = [];
  while (node || s.length) {
    if (node) {
      s.push(node);
      node = node.left;
    } else {
      const n = s.pop();
      cb(n);
      node = n.right;
    }
  }
}
```

##### 后序遍历

```js
// 非递归遍历
function mapTree2(node, cb) {
  const s = [];
  let lastVisit;
  while (node || s.length) {
    if (node) {
      s.push(node);
      node = node.left;
    } else {
      const n = s.pop();
      if (!n.right || n.right === lastVisit) {
        cb(n);
        lastVisit = n;
      } else {
        s.push(n);
      }
      node = n.right;
    }
  }
}
```

### 广度遍历(BFS)

```js
function mapTree(node, cb) {
  const queue = [node];
  while (queue.length) {
    const n = queue.shift();
    cb(n);
    if (n.left) queue.push(n.left);
    if (n.right) queue.push(n.right);
  }
}
```
