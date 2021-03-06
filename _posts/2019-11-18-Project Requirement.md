---
layout:     post
title:      "Project Requirement"
subtitle:   "今天也是bug满满的一天呀"
date:       2019-11-18
author:     "Henry"
header-img: "img/post-bg2.jpg"

tags:
    - Study Account
    - Project Requirement
---

## Project Requirement（实时更新）



- [x] 1、从单片机中获取采样数据

- [x] 2、解决为何数据将负数置零了😥

- [x] 3、解决缓存区大小，改用动态分配内存的方法😱

- [x] 4、**改变RTX实时操作系统的任务调度方式——将数据处理的工作放在任务函数中而不是中断函数中，中断函数只负责接收AD采样点**
  - [x] ① 修改信号量的方式去进行任务调度，将数据直接复制到寄存器中（取消使用队列）
  - [x] ② 将读峰峰值数据处理放到任务函数TimeProc中
    - [x] 修改读峰峰值时对max,min的初始化
    - [x] 设置阈值触发记录保存波形数据
      - [x] 1.触发阈值标志位放在modbus寄存器6中，初始化为1（置1为可更新，置0为不可更新）
      - [x] 2.STM32：一达到某个阈值（1500），单片机会触发寄存器读取保存缓冲区以及后面的数据（缓冲区尽可能大——700个点，一共7000个点），然后置0
        - [x] 判断阈值应该在每个缓冲区内（需要减去超过阈值前的数据），然后将后面的数据也要保存进去，再置0
        - [x] 修改modbus配置（读、写标志位寄存器6）
        - [x] 解决读数据全为0的问题
      - [x] 3.上位机：上位机判断标志位是否为0，若是，则保存寄存器中的数据后，标志位置1（可更新）；若不是，则等待
      - [x] **判断触发的工作应该是在中断函数里？**
    - [x] 处理峰峰值数据
  - [x] ③ 将放电量等的数据处理放到任务函数TimeProc中
  	- [x] 1、放电时间：开始——实时波形记录开始；结束：单词放电结束（采用最小放电时间间隔计算）(不作计算)
    
    - [x] 2、放电量：每次放电时间按10ms（14000个点）计算，直接绝对值积分（求和）数据
    
      ​		⚡放电量要用32位储存，然后在上位机进行转换处理——Ev/4096*3.3
    
    - [x] 3、放电次数：单次放电结束后+1
  
- [x] 5、修改上位机中动态分配内存的问题（用vector，直接将寄存器中的数据读出）

- [x] **6、解决读取波形的数据帧因Modbus协议传输慢而被覆盖部分的问题（设置合理的传输方式——缓冲区，以及传输数量和采样点**

- [x] **6.5 修改单片机和上位机的编码规范（包括命名规范、格式、注释规范）**

- [x] 7、新增功能：读取近一分钟的放电水平(将一分钟内的总电量当作这一分钟的放电水平)

  - [x] ①由于数据量过大，改为每0.5s求一次平均值，再算出累加当作这一分钟的放电强度
  - [x] ②当一分钟数据记录完后，更新modbus寄存器
  - [x] ③上位机每分钟从单片机读取一次数据，并保存到CSV中
  - [x] ④修改：0.5s求均值时少除10ms的点数，以跟单次放电作对比
  
- [x] 8、新增功能：读取近一分钟内的放电次数

- [x] 9、🔖增加及修改部分注释

- [x] 10、📃编写技术文档

  - [x] 升级局放终端方法
  - [x] Modbus寄存器定义

- [x] 11、📌添加一个通过modbus广播地址与局放终端通信修改终端地址的功能

- 修改终端地址

- 保存修改后的地址到Flash

- [x] 12、删除从文件配置的功能

- [x] 13、新增修改终端波特率的功能

  - 能修改终端波特率，但是debug上的波特率没有设置好







> Q:上一个modbus寄存器地址太长了会不会影响下一个使用
>
> A:会（22333）













> https://blog.csdn.net/u012878503/article/details/80187876 vector去重不改变顺序