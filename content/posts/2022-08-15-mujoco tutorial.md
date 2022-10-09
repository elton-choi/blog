---
title: "Mujoco Tutorial"
date: 2022-08-15T21:48:13+09:00
categories:
- robotics
tags:
- simulation
keywords:
- mujoco
#thumbnailImage: //example.com/image.jpg
---
# Main Features

- **M**ulti-**J**oint dynamics with **C**ontact
- Freely available by DeepMind in October 2021
- Key-Features
    - Generalized coordinates combined with modern contact dynamics
    - Soft, convex and analytically-invertable contact dynamics
    - Tendon geometry
    - General actuation model
    - Reconfigurable computation pipeline
    - Interactive simulation and visualization

# C/C++ Library

## Install
```bash
export LD_LIBRARY_PATH="~/mujoco/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}"
```

or you can put library file together with your execution file.

Download archive files and extract where you want.

Just add library path on your environment.

ex) edit ~/.bashrc on linux. if folder you extract is mujoco

```bash
export LD_LIBRARY_PATH="~/mujoco/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}"
```

or you can put library file together with your execution file.

**Test Execution**

```bash
./simulate ../model/humanoid/humanoid.xml
```

## **Simple Tutorial**

{{< youtubepl "PLc7bpbeTIk758Ad3fkSywdxHWpBh9PM0G" >}}


# Python Bindings

Mujoco provides python bindings officially supported from 2.1.2.

Before this, there are previous python bindings such as mujoco-py not officially supported.

## Install

```bash
pip install mujoco
```

## **Simple Example**
[https://github.com/BolunDai0216/PyMuJoCoBase](https://github.com/BolunDai0216/PyMuJoCoBase)

```bash
import mujoco
import glfw

def init_window(max_width, max_height):
    glfw.init()
    window = glfw.create_window(width=max_width, height=max_height,
                                       title='Demo', monitor=None,
                                       share=None)
    glfw.make_context_current(window)
    return window

window = init_window(1200, 900)

model = mujoco.MjModel.from_xml_path('../model/humanoid/humanoid.xml')
# model = mujoco.MjModel.from_xml_path('../model/ur5/ur5.xml')
# model = mujoco.MjModel.from_xml_path('../model/PR2/pr2.xml')

data = mujoco.MjData(model)
context = mujoco.MjrContext(model, mujoco.mjtFontScale.mjFONTSCALE_100)

width, height = glfw.get_framebuffer_size(window)
viewport = mujoco.MjrRect(0, 0, width, height)

scene = mujoco.MjvScene(model, 1000)
camera = mujoco.MjvCamera()
mujoco.mjv_updateScene(
    model, data, mujoco.MjvOption(), mujoco.MjvPerturb(),
    camera, mujoco.mjtCatBit.mjCAT_ALL, scene)

while(not glfw.window_should_close(window)):
    mujoco.mj_step(model, data)

    mujoco.mjv_updateScene(
        model, data, mujoco.MjvOption(), None,
        camera, mujoco.mjtCatBit.mjCAT_ALL, scene)
    mujoco.mjr_render(viewport, scene, context)

    glfw.swap_buffers(window)
    glfw.poll_events()

glfw.terminate()
```
## Simple Tutorial
{{< youtubepl "PLc7bpbeTIk75dgBVd07z6_uKN1KQkwFRK" >}}

# Reference

[1] [MuJoCo Bootcamp](https://pab47.github.io/mujoco.html)

[2] [MuJoCo](https://mujoco.org/)

[3] [https://github.com/deepmind/mujoco](https://github.com/deepmind/mujoco)

[4] [studywolf](https://studywolf.wordpress.com/)