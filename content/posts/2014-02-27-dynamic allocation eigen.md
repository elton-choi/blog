---
title: "Dynamic allocation of Eigen in hard-real time environment"
date: 2014-02-27T21:48:13+09:00
categories:
- programming
- c++
tags:
- rtos
- eigen
keywords:
- eigen
#thumbnailImage: //example.com/image.jpg
---

Eigen is a open-source, template-based, c++, matrix library.
It has good balance between performance and ease-of-use.
We can write matrix formulation intuitively as similar as in Matlab.

Futhermore, Eigen has Geometry module which others don't have.
Especially, I like this feature since I can use quaternion, angle-axis, transformation matrix in robotics application.

However, though above good features,
We could get unexpected behavior if we don't take care of using Eigen in hard real-time environment.
One point we have to avoid is a dynamic memory allocation.
Since we don't know how many memory needed when allocated, and it takes time to take heap memory up and down, so it can break real-time loop.

When you declare matrix in Eigen, the below is the simplest way. 
```cpp
MatrixXf A;
```
MatrixXf is typedef of Eigen. It is same as
```cpp
Matrix<float, Dynamic, Dynamic> A;
```
The second and thrid argument is row and column size of matrix, respectively. If you declare eigen matrix variable like this, you can resize matrix A as you want. However, the memories for this are dynamically allocated in the backgroud whenever you resize in your application.

Eigen provides fixed-size matrix declaration to avoid this.
```cpp
Matrix3f A;
```
A is 3-by-3 matrix, and is same as
```cpp
Matrix<float, 3, 3> A;
```
I recommend that you should use fixed-size matrix in the hard-real time environment. However, what if you don't know the exact size of matrix?
For this case, Eigen provideto
You can use 10-by-10 memory stack and limit the size of matrix by row_size and col_size dynamically.
I think stl vector reserve method is similar with this.

In sum, you have to avoid dynamic allocation in the hard real-time environment. To do that, you should carefully use matrix declaration of Eigen library.