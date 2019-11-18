---
layout:     post
title:      "Project Requirement"
subtitle:   "今天也是bug满满的一天呀"
date:       2019-11-18
author:     "Henry"

tags:
    - Study Account
    - Project Requirement
---

## Project Requirement（实时更新）



- [x] 1、从单片机中获取采样数据
- [x] 2、解决为何数据将负数置零了😥
- [x] 3、解决缓存区大小，改用动态分配内存的方法😱
- [ ] 4、**改变RTX实时操作系统的任务调度方式——将数据处理的工作放在任务函数中而不是中断函数中，中断函数只负责接收AD采样点**
  - [x] ① 修改信号量的方式去进行任务调度，将数据直接复制到寄存器中（取消使用队列）
  - [x] ② 将读峰峰值数据处理放到任务函数TimeProc中
  - [ ] ③ 修改读峰峰值时对max,min的初始化
  - [ ] ④ 将放电量等的数据处理放到任务函数TimeProc中
- [ ] 5、修改上位机中动态分配内存的问题（用vector，直接将寄存器中的数据读出）
- [ ] 6、解决读取波形的数据帧因Modbus协议传输慢而被覆盖部分的问题（设置合理的传输方式——缓冲区，以及传输数量和采样点）