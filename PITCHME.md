## 500 行 Python 写一个渲染器

### by 谭啸

---

## 实现步骤

+++

@snap[west span-50]
### 1, 2d 平面上画点、线、三角形，填充三角形
@snapend

@snap[east span-50]
![](bresenham.png)
@snapend


+++

@snap[north span-100]
#### Bresenham 画线算法
@snapend

@snap[west span-70]
```python
def draw_line(
    v1: Vec2d, v2: Vec2d, canvas: Canvas
):
    slope = abs((v1.y - v2.y) / (v1.x - v2.x))
    y = v1.y
    err = 0
    incr = 1 if v1.y < v2.y else -1
    dots = []
    for x in range(v1.x, v2.x):
        dots.append(x, y)
        err += slope
        if abs(error) >= 0.5:
            y += incr
            error -= 1

    canvas.draw(dots)
```
@snapend

@snap[east span-30]
![](bresenham.png)
@snapend

+++

@snap[west span-50]
### 2, 3d 顶点坐标转换为2d平面坐标
@snapend

@snap[east span-50]
![](monkey_wireframe.png)
@snapend

+++

@snap[north span-100]
### 矩阵变化
@snapend

@snap[west span-60]
```python
# the render pipeline
projection_matrix * view_matrix *\
    world_vertex * model.vertex
```
@snapend

@snap[east span-40]
![](monkey_wireframe.png)
@snapend

+++
@snap[west span-50]
### 4, 简单的光照
@snapend


@snap[east span-50]
![](monkey_zbuffer.png)
@snapend


+++
@snap[west span-50]
### 5,  纹理映射
@snapend

@snap[east span-50]
![](axe.png)
@snapend

---

## 经验

* 按图形学发展过程来做
* 每一步实现，看到反馈
* 找到正确的google关键词


---


## Github 地址

https://github.com/tvytlx/render-py
