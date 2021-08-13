整理一下CSS3D相关的接口,其实可以实现很多有意思的东西

### 基础

与CSS3D相关的属性有以下几个

| 属性                | 值                                                           | 描述                                                     |
| ------------------- | ------------------------------------------------------------ | -------------------------------------------------------- |
| transform           | translate3D(x,y,z) , scale3D(x,y,z),rotate3D(x,y,z,angle)... | 用来变换元素,这里只说与3d相关的属性                      |
| transform-origin    | (x, y, z), 值可以是 left, center, right ,length,%            | 用来设置 转换元素时的相对位置                            |
| transform-style     | 默认 flat, 表示所有子元素在2d平面上呈现<br />preserve-3d 表示所有子元素在3d空间呈现 | 这个属性关键啊                                           |
| perspective         | number \| none                                               | 这个可以理解为你的眼睛观察origin位置的距离离得近看的就大 |
| perspective-origin  | x-axis, y-axis 值有 left center right length %               | 配合上面使用的                                           |
| backface-visibility | visible , hidden                                             | 定义元素背面是否可见                                     |

### 案例

直接实现一个小demo更加直观