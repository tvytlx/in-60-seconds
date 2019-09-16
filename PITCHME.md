## 500 行 Python 写一个渲染器

### by 谭啸

---

## 实现步骤

+++

@snap[west span-50]
### 2d 平面上画点、线、三角形，填充三角形
@snapend

@snap[east span-50]
![](bresenham.png)
@snapend


+++

@snap[north span-100]
#### 2d 平面画线算法
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
### 3d 顶点坐标转换为2d平面坐标，连线
@snapend

@snap[east span-50]
![](monkey_wireframe.png)
@snapend

+++

@snap[north span-100]
### 3d 顶点坐标转换为2d平面坐标，连线
@snapend

@snap[west span-70]
```python
world_vertices = []

def mvp(v):
    model_matrix = Mat4d([[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]])
    view_matrix = look_at(Vec3d(-4, -4, 10), Vec3d(0, 0, 0))
    projection_matrix = perspective_project(0.5, 0.5, 3, 1000)
    world_vertex = model_matrix * v
    world_vertices.append(Vec4d(world_vertex))
    return projection_matrix * view_matrix * world_vertex

def ndc(v):
    """
    各个坐标同时除以 w，得到 NDC 坐标
    """
    v = v.value
    w = v[3, 0]
    x, y, z = v[0, 0] / w, v[1, 0] / w, v[2, 0] / w
    return Mat4d([[x], [y], [z], [1 / w]])

def viewport(v):
    x = y = 0
    w, h = width, height
    n, f = 0.3, 1000
    return Vec3d(
        w * 0.5 * v.value[0, 0] + x + w * 0.5,
        h * 0.5 * v.value[1, 0] + y + h * 0.5,
        0.5 * (f - n) * v.value[2, 0] + 0.5 * (f + n),
    )

# the render pipeline
screen_vertices = [viewport(ndc(mvp(v))) for v in model.vertices]
```
@snapend

@snap[east span-30]
![](monkey_wireframe.png)
@snapend

+++
@snap[west span-50]
### 简单的光照
@snapend


@snap[east span-50]
![](monkey_zbuffer.png)
@snapend


+++
@snap[west span-50]
### 纹理映射
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
