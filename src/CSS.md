# position



# 水平居中

- 子元素是内联元素，父元素设置：

  ```css
  text-align: center;
  ```

- 子元素是块级元素，子元素且设置了宽度，子元素设置：

  ```css
  margin: 0 auto;
  ```

  如果没有给子元素设置宽度，那么子元素的宽度与父元素的宽度相等



# 垂直居中

- 子元素是一行内联元素，父元素没有设置高度，父元素设置：

  ```css
  padding-top: 10px;
  padding-bottom: 10px;
  ```

  或：

  父元素已设置高度，设置`line-height`的值等于`height`的值：

  ```css
  height: 100px;
  line-height: 100px;
  ```

- 子元素是多行内联元素，上述的padding方法也是可用的，也可以使用这个：

  ```css
  .center-table {
    display: table;
    height: 250px;
    width: 240px;
  }

  .center-table p {
    display: table-cell;
    vertical-align: middle;
  }
  ```

- 子元素是块级元素，并且知道其高度，可以这样设置：

  ```css
  .main {
    width: 300px;
    height: 200px;
    position: relative;
  }

  .main > div {
    position: absolute;
    top: 50%;
    height: 100px;
    margin-top: -50px;	// margin-top的值是该高度的一半
  }
  ```

  或者这样：

  ```css
  .main {
    width: 300px;
    height: 200px;
    position: relative;
  }

  .main > div {
    width: 200px;
    height: 150px;
    position: absolute;
    top: 0; 
    bottom: 0;
    margin: auto;
  }
  ```

  如果想要块级元素水平垂直居中：

  ```css
  .main {
    width: 300px;
    height: 200px;
    position: relative;
  }

  .main > div {
    width: 200px;
    height: 150px;
    position: absolute;
    top: 0; 
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
  }
  ```

- 子元素是块级元素，但不知道其高度，可以这样设置：

  ```css
  .main {
    width: 300px;
    height: 200px;
    position: relative;
  }

  .main > div {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);	// 这一步是重点
  }
  ```

  ​