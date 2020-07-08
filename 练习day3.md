### 题目要求：获取上一个哥哥元素节点的方法，previousElementSibling不能使用，previousSibling可以使用

#### 伪代码：

1. 获取 ele的哥哥节点，previousSibling属性可以获取到，如果ele没有哥哥节点返回的是null

2. 如果哥哥节点是null，那元素哥哥节点更不要说了，肯定也是null，直接返回一个null就好了；比如下面例子的 \<li>1\</li>就没有哥哥节点

3. 如果有哥哥节点，那我们就判断一下这个哥哥节点的nodeType是否是1，是的话，就返回；不是那我们就继续找，看哥哥的哥哥看是否为1，哥哥的哥哥的哥哥，依次类推，直到没有哥哥了（也就是为null）了，那好吧他就是没有哥哥节点，退出寻找
    + 说的有些抽象，还是以下面这段HTML结构的\<li>3\</li>为例
    + 第一步：\<li>3\</li>的哥哥是\<!-- 注释三 -->，但\<!-- 注释三 -->的nodeType不是1，继续找它的哥哥
    + 第二步：\<!-- 注释三 -->的哥哥是\<!-- 注释二 -->，但\<!-- 注释二 -->的nodeType也不是1，继续找他的哥哥
    + 第三步：\<!-- 注释二 -->的哥哥是\<li>2\</li>，\<li>2\</li>的nodeType是1，ok，结束寻找

+ ```html
    <ul>
    	<li>1</li>
      <!-- 注释一 -->
      <li>2</li>
      <!-- 注释二 -->
      <!-- 注释三 -->
      <li>3</li>
    </ul>
    ```

4. 那如何 一直找（也就是一直循环），但是满足条件了就不找了（也就是退出循环），像这种情况不知道循环次数的，就用while，while的()里面写满足的条件
5. 将 找到的哥哥节点（没有就是null）return出去，供外部获取

#### 代码实现

```javascript
function prev(ele){
  let siblingNode = ele.previousSibling;
  while(siblingNode && siblingNode.nodeType!==1){
    // 一直循环，直到获得的哥哥节点为null（也就是ele根本没有哥哥节点），或者哥哥节点的nodeType=1就退出循环，
    siblingNode = siblingNode.previousSibling;
  }
  return siblingNode;
}
let res = prev(ele);
```







### 题目要求：获取所有元素子节点，但不能使用children，可以使用childNodes。

#### 伪代码：

1. 准备一个存放所有元素子节点的【“容器”】，应该是一个数组；如果ele没有子节点，返回[];
2. chilNodes属性特点
    + 通过chilNodes属性可以获得 元素的所有子节点，包含：注释节点、文本节点、元素节点等其他节点，注释节点、文本节点、元素节点是我们常见的节点类型；
    + 同时ele.childNodes返回的是一个“类数组”，我们可以像数组一样操作它，比如它有length，可以for循环
    + 如果这个ele没有子节点，那么它的childNodes属性就是一个“空数组”
3. 从【所有子节点】中筛选出元素节点。
    + 而区分节点的类型，靠的就是nodeType这个属性，我们这里要筛选的元素节点的nodeType就是1；
    + nodeType是1的话我们就把它放到【第一步的容器中】
4. 因为知道要循环多少次，【所有子节点.length】次，所以用for循环
5. 将【第一步的容器】return出去，供外部获取

#### 代码实现

```javascript
function childs(ele){
  let eleChildren = [];//1
  let eleChildNodes = ele.childNodes;//2
  for(let i=0; i< eleChildNodes.length; i++){//4
    // 3.
    let curNode = eleChildNodes[i];
    if(curNode.nodeType===1){
      eleChildren.push(curNode);// 使用数组的push方法，把是元素节点的丢进我们的“容器”中
    }
  }
  return eleChildren;//5
}
let res = childs(ele);
```

