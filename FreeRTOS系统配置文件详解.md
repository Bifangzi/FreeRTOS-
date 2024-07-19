# FreeRTOS系统配置文件详解

---

## FreeRTOSConfig.h文件概述

考虑到使用场景的不同，可以通过FreeRTOSConfig.h文件对操作系统进行配置和裁剪，API函数的使能以节约单片机中宝贵的内存资源。
FreeRTOSConfig.h文件中主要包含以下内容：

- “INCLUDE”配置项
- “config”配置项
- 其他配置项

---

## “config”配置项

完成对FreeRTOS的功能配置和裁剪



### 基础配置项

*任务调度模式选择*

```c
#define configUSE_PREEMPTION                            1 
```

0：协程式调度器

1：抢占式调度器

无默认定义



*任务调度方式*

```c
#define configUSE_PORT_OPTIMISED_TASK_SELECTION         1
```

0：使用软件算法计算下一个需要运行的任务，该方法具有通用性，不限制使用硬件，并且**不限制任务优先级最大值**，但是该方法效率较低

1：使用硬件方法计算下一个运行任务，基于特定架构下的汇编指令（类计算前导置零），该方法并不支持所有硬件，**任务优先级最大通常为32**

默认为0



*使能ticklsess低功耗模式*

```c
#define configUSE_TICKLESS_IDLE                         0
```

0：失能tickless低功耗模式

1：使能tickless低功耗模式

该设置不适用所有硬件

默认为0



*定义CPU主频*

```c
#define configCPU_CLOCK_HZ                              SystemCoreClock
```

设置CPU主频，单位位Hz

无默认定义



*定义系统SysTick时钟频率*

```c
#define configSYSTICK_CLOCK_HZ                          (configCPU_CLOCK_HZ / 8)
```

系统SysTick时钟频率与内核时钟主频不同时才定义，单位为Hz

默认不定义

**该宏只在F1系列中开启，在F4、F7和H7中被屏蔽**，F1系统SysTick定时频率被8分频



*系统时钟节拍频率*

```c
#define configTICK_RATE_HZ                              1000
```

定义系统节拍频率，单位为Hz

无默认定义

>         任何操作系统都需要提供一个时钟节拍，以供系统处理诸如延时，超时等与时间相关的事件。时钟节拍是特定的周期性中断， 这个中断可以看做是系统心跳。 中断之间的时间间隔取决于不同的应用，一般是 1ms – 100ms。时钟的节拍中断使得内核可以将任务延迟若干个时钟节拍，以及当任务等待事件发生时，提供等待超时等依据。时钟节拍率越快，系统的额外开销就越大。
>         对于 Cortex-M3 内核的 STM32F103 和 Cortex-M4 内核的 STM32F407 以及 F429，都是用滴答定时器来实现系统时钟节拍的。



*最大优先级数量*

```c
#define configMAX_PRIORITIES                            32
```

定义最大优先级数量，最大优先级为 configMAX_PRIORITIES-1

无默认定义



*空闲任务栈大小*

```c
#define configMINIMAL_STACK_SIZE                        128
```

定义空闲任务栈大小，单位为word，1字=2字节

无默认定义

>         空闲任务是FreeRTOS不可缺少的任务，因为FreeRTOS设计要求必须至少有一个任务处于运行状态。当RTOS调度器开始工作后，为了保证至少有一个任务在运行，空闲任务被自动创建，占用最低优先级（0优先级）。



*任务名最大字符数*

```c
#define configMAX_TASK_NAME_LEN                         16
```

定义任务名最大字符数,

默认: 16



*定义系统时钟节拍计数器的数据类*

```c
#define configUSE_16_BIT_TICKS                          0
```

定义系统时钟节拍计数器的数据类型

0：32位无符号整形

1：16位无符号整形

无默认定义



*同优先级任务抢占空闲任务*

```c
#define configIDLE_SHOULD_YIELD                         1
```

使能在**抢占式调度模式**，同优先级的任务能抢占空闲任务

0：不允许0优先级任务抢占空闲任务（0优先级）

1：允许0优先级任务抢占空闲任务（0优先级）

默认为1



*任务间直接的消息传递*

```c
#define configUSE_TASK_NOTIFICATIONS                    1
```

使能任务间直接的消息传递,包括信号量、事件标志组和消息邮箱，，开启后每个任务将多占用8字节内存空间

0：失能直接消息传递

1：使能直接消息传递

默认为1



*定义任务通知数组的大小*

```c
#define configTASK_NOTIFICATION_ARRAY_ENTRIES
```

此宏用于定于任务通知数组的大小

默认为1



*使能互斥信号量*

```c
#define configUSE_MUTEXES                               1
```

0：失能互斥信号量

1：失能互斥信号量

默认为0



*使能递归互斥信号量·*

```c
#define configUSE_RECURSIVE_MUTEXES                     1
```

0：失能递归互斥信号量

1：使能递归互斥信号量

默认为0



*使能计数信号量*

```c
#define configUSE_COUNTING_SEMAPHORES                   1
```

0：失能计数信号量

1：使能计数信号量

默认为0



*可注册信号量及消息队列个数*

```c
#define configQUEUE_REGISTRY_SIZE                       8
```

此宏用于定义可以注册的队列和信号量的最大数量，此宏定义仅用于调试使用

默认为0



*使能队列集*

```c
#define configUSE_QUEUE_SETS                            1
```

0：失能队列集

1：使能队列集



*使能时间片调度*

```c
#define configUSE_TIME_SLICING                          1
```

0：失能时间片调度

1：**使用抢占式调度**时使能时间片调度

默认为1



*分配Newlib的重入结构体*

```c
#define configUSE_NEWLIB_REENTRANT                      0
```

此 宏 用 于 为 每 个 任 务 分 配 一 个 NewLib 重 入 结 构 体

0：任务创建时不分配NewLib结构体

1：任务创建时向任务控制块中分配一个NewLib重入结构体

默认为0



*兼容老版本*

```c
#define configENABLE_BACKWARD_COMPATIBILITY             0
```

此宏用于兼容 FreeRTOS 老版本的 API 函数

0：不兼容

1：兼容老版本API函数

默认为1



*定义线程本地存储指针的个数*

```c
#define configNUM_THREAD_LOCAL_STORAGE_POINTERS         0
```

此宏用于在任务控制块中分配一个线程本地存储指着数组

\>1：``configNUM_THREAD_LOCAL_STORAGE_POINTERS``为线程本地存储指针数组的元素个数

0：禁用线程本地存储指针数组



*深度及长度数据类型*

```c
#define configSTACK_DEPTH_TYPE                          uint16_t
#define configMESSAGE_BUFFER_LENGTH_TYPE                size_t 
```

用于定义任务堆栈深度的数据类型，默认为uint16_t

用于定义消息缓冲区中消息长度的数据类型，默认为size_t

---

### 内存分配项



*静态与动态内存申请*

```c
#define configSUPPORT_STATIC_ALLOCATION                 0 
#define configSUPPORT_DYNAMIC_ALLOCATION                1
```

0：不支持静态/动态内存申请

1：支持静态/动态内存申请

默认启用动态内存申请



*动态管理大小*

```c
#define configTOTAL_HEAP_SIZE                           ((size_t)(10 * 1024))
```

FreeRTOS堆中可用的RAM总量, 单位: Byte

无默认定义



*手动分配内存堆*

```c
#define configAPPLICATION_ALLOCATED_HEAP                0
```

0：内存堆由编译器进行分配

1：用户自行建立内存堆

默认为0



*手动申请内存与释放*

```c
#define configSTACK_ALLOCATION_FROM_SEPARATE_HEAP       0
```

0：编译器自行申请与释放内存

1：用户自行执行内存申请与释放

默认为0

---

### 钩子函数（回调函数）配置



*空闲任务钩子函数*

```c
#define configUSE_IDLE_HOOK                             0
```

此宏用于使能空闲任务的钩子函数

0：使能空闲任务的钩子函数

1：失能空闲任务的钩子函数

无默认定义



*系统时钟节拍中断钩子函数*

```c
#define configUSE_TICK_HOOK                             0
```

0：失能系统节拍中断钩子函数

1：使能系统节拍中断钩子函数

无默认定义



*栈溢出检测*

```c
#define configCHECK_FOR_STACK_OVERFLOW                  0
```

0：不对栈溢出检测

1：使用方法1检测栈溢出

2：使用方法2检测栈溢出

默认为0

> **方法1：** 在每次RTOS内核进行上下文的切换时，任务的堆栈可能会达到最大值。因此，内核检测堆栈指针(stack poiter)来判断当前堆栈是否溢出。这种方法很快速，但是无法保证捕捉到所有的堆栈溢出。
> 
> **方法2：** 在每个任务创建时，将它的堆栈用特定数值填充。因此，当堆栈没有溢出时，堆栈最后的内存数据应该始终是初始的填充值。FreeRTOS通过检测堆栈的最后16个字节的数据是否发生改变，来确定堆栈是否溢出。方法2是在方法1的基础上完成的，无法单独使用方法2。
> 
> **值得注意的是这两种方法均无法保证捕捉所有的堆栈溢出**



*动态内存分配失败钩子函数*

```c
#define configUSE_MALLOC_FAILED_HOOK                    0 
```

0：失能动态内存分配失败钩子函数

1：使能动态内存分配失败钩子函数

默认为0



*定时器服务任务首次执行前的钩子函数*

```c
#define configUSE_DAEMON_TASK_STARTUP_HOOK              0
```

0：使能定时器任务首次执行前的钩子函数

1：使能定时器任务首次执行前的钩子函数

默认为0

---

### 运行时间及任务统计配置



*任务运行时间统计*

```c
#define configGENERATE_RUN_TIME_STATS                   0
```

1：统计任务运行时间

0：不统计任务运行时间

默认为0

```c
#if configGENERATE_RUN_TIME_STATS
#include "./BSP/TIMER/btim.h"
#define portCONFIGURE_TIMER_FOR_RUN_TIME_STATS()        ConfigureTimeForRunTimeStats()
extern uint32_t FreeRTOSRunTimeTicks;
#define portGET_RUN_TIME_COUNTER_VALUE()                FreeRTOSRunTimeTicks
#endif
```

`ConfigureTimeForRunTimeStats()`为定时器初始化函数，系统会自动调用该函数初始化时基

`FreeRTOSRunTimeTicks`用以记录定时器中断次数



*可视化跟踪调试*

```c
#define configUSE_TRACE_FACILITY                        1
```

0：失能可视化跟踪调试

1：使能可视化跟踪调试

默认为0



*调试任务相关函数编译*

```c
#define configUSE_STATS_FORMATTING_FUNCTIONS            1
```

该宏与`configUSE_TRACE_FACILITY`均定义为1时，启用编译函数`vTaskList()`及`vTaskGetRunTimeStats()`

- `vTaskList()`用于获取任务的相关信息，如名字，堆栈大小等

- `vTaskGetRunTimeStats()`用于获取任务运行时间

0：不编译上述两个函数

默认为0

---

### 协程相关配置



*启用协程*

```c
#define configUSE_CO_ROUTINES                           0
```

0：不启用协程

1：启用协程

默认为0



*协程最大优先级数量*

```c
#define configMAX_CO_ROUTINE_PRIORITIES                 2
```

协程最大优先级数值为configMAX_CO_ROUTINE_PRIORITIES-1

无默认定义，configUSE_CO_ROUTINES定义为1时需要定义

---

### 软件定时器相关配置



*软件定时器启用*

```c
#define configUSE_TIMERS                                1
```

0：不启用软件定时器

1：启用软件定时器

默认为0



*软件定时器优先级*

```c
#define configTIMER_TASK_PRIORITY                       ( configMAX_PRIORITIES - 1 )
```

软件定时器的优先级，无默认定义，`configUSE_TIMERS`定义为1时需要定义，此处定义为最高优先级



*软件定时器队列长度*

```c
#define configTIMER_QUEUE_LENGTH                        5
```

无默认定义，`configUSE_TIMERS`定义为1时需要定义



*软件定时器任务栈空间大小*

```c
#define configTIMER_TASK_STACK_DEPTH                    ( configMINIMAL_STACK_SIZE * 2)
```

软件定时器处理任务的栈空间大小，无默认定义，`configUSE_TIMERS`定义为1时需要定义，此处定义为空闲任务的两倍，单位为word

---

### 中断嵌套相关配置



*优先级寄存器位数*

```c
#ifdef __NVIC_PRIO_BITS
    #define configPRIO_BITS __NVIC_PRIO_BITS
#else
    #define configPRIO_BITS 4
#endif
```

此宏用于定义MCU的8位优先级配置寄存器实际使用位数，默认为4



*中断最低/最高优先级*

```c
#define configLIBRARY_LOWEST_INTERRUPT_PRIORITY         15
#define configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY    5
```

> 对于 STM32，在使用 FreeRTOS 时，建议将中断优先级分组设置为组 4，此时中断的最低优先级为 15。当中断的优先级数值小于configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY 时，此中断不受 FreeRTOS 管理。



*中断最低/最高优先级在寄存器中的数值*

```c
#define configKERNEL_INTERRUPT_PRIORITY                 ( configLIBRARY_LOWEST_INTERRUPT_PRIORITY << (8 - configPRIO_BITS) )
#define configMAX_SYSCALL_INTERRUPT_PRIORITY            ( configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY << (8 - configPRIO_BITS) )
#define configMAX_API_CALL_INTERRUPT_PRIORITY           configMAX_SYSCALL_INTERRUPT_PRIORITY
```

> MCU 的最低中断优先等级在中断优先级配置寄存器中的值，对于 STM32，即configLIBRARY_LOWEST_INTERRUPT_PRIORITY 偏移 4bit 的值。
> 
> FreeRTOS 可管理中断的最高优先等级在中断优先级配置寄存器中的值，对于 STM32，即宏 configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY 偏移 4bit 的值。
> 
> 后两个宏等价

---

### 断言

>          FreeRTOS中的断言函数configASSERT()和标准C中的断言函数assert()是一样的，如果断言函数的参数为0时将触发断言函数的执行。 
> 
> 　　FreeRTOS的断言功能在调试阶段是非常有用的，可以有效地检查参数错误和运行中的错误，但在正式发布软件时，请将此功能关闭，因为断言功能会增加工程代码大小并降低工程执行效率。关闭断言也比较简单，如果FreeRTOSConfig.h文件中有断言的宏定义，将其注释掉即可，如果没有宏定义，默认在FreeRTOS.h文件中就是关闭的。

```c
#define vAssertCalled(char, int) printf("Error: %s, %d\r\n", char, int)
#define configASSERT( x ) if( ( x ) == 0 ) vAssertCalled( __FILE__, __LINE__ )
```

- `vAssertCalled(char, int) `用于配置宏` configASSERT(x)`通过串口输出断言信息

- `configASSERT(x)`用于判断`x`真假，`x`为假时表明程序出错然后调用`vAssertCalled(char, int) `输出错误信息

*使用断言将会在程序运行时检测错误，会增加执行开销，应该在程序调试完成后将断言屏蔽*

---



### MPU特殊定义

MPU：Memory Protection Unit，即内存保护单元，MPU是Cortex-M的选配件，STM32F1系列中，只有STM32F10X_XL系列的芯片才具有这个MPU存储保护单元，而其他STM32F1芯片没有。

MPU的主要功能是对内存故障，内存溢出的错误进行保护，提高嵌入式系统的稳定性。由于现阶段使用的F1系列芯片不含MPC故不展开，待日后完善。

```c
/* FreeRTOS MPU 特殊定义 */
//#define configINCLUDE_APPLICATION_DEFINED_PRIVILEGED_FUNCTIONS 0
//#define configTOTAL_MPU_REGIONS                                8
//#define configTEX_S_C_B_FLASH                                  0x07UL
//#define configTEX_S_C_B_SRAM                                   0x07UL
//#define configENFORCE_SYSTEM_CALLS_FROM_KERNEL_ONLY            1
//#define configALLOW_UNPRIVILEGED_CRITICAL_SECTIONS             1
```

---

## “INCLUDE“配置项

配置FreeRTOS中可选的API函数

```c
#define INCLUDE_vTaskPrioritySet                        1                 /* 设置任务优先级 */
#define INCLUDE_uxTaskPriorityGet                       1                 /* 获取任务优先级 */
#define INCLUDE_vTaskDelete                             1                 /* 删除任务 */
#define INCLUDE_vTaskSuspend                            1                 /* 挂起任务 */
#define INCLUDE_xResumeFromISR                          1                 /* 恢复在中断中挂起的任务 */
#define INCLUDE_vTaskDelayUntil                         1                 /* 任务绝对延时 */
#define INCLUDE_vTaskDelay                              1                 /* 任务延时 */
#define INCLUDE_xTaskGetSchedulerState                  1                 /* 获取任务调度器状态 */
#define INCLUDE_xTaskGetCurrentTaskHandle               1                 /* 获取当前任务的任务句柄 */
#define INCLUDE_uxTaskGetStackHighWaterMark             1                 /* 获取任务堆栈历史剩余最小值 */
#define INCLUDE_xTaskGetIdleTaskHandle                  1                 /* 获取空闲任务的任务句柄 */
#define INCLUDE_eTaskGetState                           1                 /* 获取任务状态 */
#define INCLUDE_xEventGroupSetBitFromISR                1                 /* 在中断中设置事件标志位 */
#define INCLUDE_xTimerPendFunctionCall                  1                 /* 将函数的执行挂到定时器服务任务*/
#define INCLUDE_xTaskAbortDelay                         1                 /* 中断任务延时 */
#define INCLUDE_xTaskGetHandle                          1                 /* 通过任务名获取任务句柄 */
#define INCLUDE_xTaskResumeFromISR                      1                 /* 恢复在中断中挂起的任务 */
```

0：失能

1：使能

---

## 其他配置项



*PendSV和SVC中断服务函数*

```c
#define xPortPendSVHandler                              PendSV_Handler
#define vPortSVCHandler                                 SVC_Handler
```

这两个宏为 PendSV 和 SVC 的中断服务函数，主要用于 FreeRTOS 操作系统的任务切换，有关 FreeRTOS 操作系统中任务切换的相关内容



*安全侧配置项*

```c
/* ARMv8-M 安全侧端口相关定义。 */
//#define secureconfigMAX_SECURE_CONTEXTS         5
```

参阅[ARMV8-M中的TrustZone如何保护代码的安全?](https://blog.csdn.net/ybhuangfugui/article/details/117793988)
