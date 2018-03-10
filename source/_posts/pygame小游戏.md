---
title: pygame小游戏
date: 2018-02-19 21:37:52
categories:
	-python
	-pygame
tags:
	-python
	-pygame
---

{% note info%}
最近了解了pygame库，根据学习内容，完成了这个小游戏，游戏大概是窗口中的文字可以移动，
上下左右键可以控制移动的速度，鼠标也可以控制文字，当窗口最小化时，文字不移动，当窗口
重新打开亦可以移动，移动时背景色发生相应的变化！不足的时鼠标控制有点问题！
{% endnote %}

## 依赖：

- pygame第三方库

- 执行 <span id="inline-blue">pip install pygame</span> 即可以安装

##代码：
```python
import pygame, sys
import pygame.freetype

# 初始化
pygame.init()
size = width, height = 600, 400#窗口大小
speed = [1, 1]#速度
GOLD = 255, 251, 0#文字颜色
aqqje = [230, 160]#文字范围
screen = pygame.display.set_mode(size, pygame.RESIZABLE)
f1 = pygame.freetype.Font('C:\Windows\Fonts\simhei.ttf', 36)#设置字体
f1rect = f1.render_to(screen, aqqje, r"aqqje'note", fgcolor=GOLD, size = 61)#绘制文字
fps = 300
flock = pygame.time.Clock()#设置帧率
bgcolor = pygame.Color('black')
still = False
def RGBChannel(a):
    return 0 if a < 0 else (255 if a > 255 else int(a))
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        #键盘控制
        elif event.type == pygame.KEYDOWN:
            #left速度+1
            if event.key == pygame.K_LEFT:
                speed[0] = speed[0] if speed[0] == 0 else (abs(speed[0]) - 1) * int(speed[0] / abs(speed[0]))
            #right速度-1
            elif event.key == pygame.K_RIGHT:
                speed[0] = speed[0] + 1 if speed[0] > 0 else speed[0] - 1
            #up速度+1
            elif event.key == pygame.K_UP:
                speed[1] = speed[1] + 1 if speed[1] > 0 else speed[1] - 1
            #down速度-1
            elif event.key == pygame.K_DOWN:
                speed[1] = speed[1] if speed[1] == 0 else (abs(speed[1]) - 1) * int(speed[1] / abs(speed[1]))
            #Esc退出
            elif event.key == pygame.K_ESCAPE:
                sys.exit()
        #改变窗口大小
        elif event.type == pygame.VIDEORESIZE:
            size = width, height = event.w, event.h
            screen = pygame.display.set_mode(size, pygame.RESIZABLE)
        #鼠标事件
        elif event.type == pygame.MOUSEBUTTONDOWN:
            if event.button == 1:
                still = True
        elif event.type == pygame.MOUSEBUTTONUP:
            still = False
            if event.pos == 1:
                aqqje[0] = event.pos[0] - f1rect.width
                aqqje[1] = event.pos[1] - f1rect.height
        elif event.type == pygame.MOUSEMOTION:
            if event.buttons[0] == 1:
                aqqje[0] = event.pos[0] - f1rect.width
                aqqje[1] = event.pos[1] - f1rect.height
    #保证不出右界
    if aqqje[0] < 0 or aqqje[0] + f1rect.width > width:
        speed[0] = -speed[0]
        if f1rect.width > width and f1rect.width + aqqje[0] > f1rect.width:
           speed[0] = -speed[0]
    #保证不出下界
    if aqqje[1] < 0 or aqqje[1] + f1rect.height > height:
        speed[1] = -speed[1]
        if f1rect.height > height and f1rect.height + aqqje[1] > f1rect.height:
           speed[1] = -speed[1]
    #窗口最小化停止移动
    if pygame.display.get_active():
        #横向移动
        aqqje[0] = aqqje[0] + speed[0]
        #向下移动
        aqqje[1] = aqqje[1] + speed[1]
    #背景可变
    bgcolor.r = RGBChannel(f1rect.width * 255 / width)
    bgcolor.g = RGBChannel(f1rect.height * 255 / height)
    bgcolor.b = RGBChannel(min(aqqje[0], aqqje[1] * 255 / max(aqqje[0], aqqje[1], 1)))
    #刷新
    screen.fill(bgcolor)
    f1rect = f1.render_to(screen, aqqje, r"aqqje'note", fgcolor=GOLD, size=61)
    pygame.display.update()
    flock.tick(fps)
```
