## 进程

### 1. 进程的概念
进程是一个具有独立功能的**程序**在一个**数据集合**上运行的过程，是**系统进行资源分配和调度的一个独立单位**

### 2. 进程控制块PCB
系统通过PCB获取进程的基本情况和运行状态，从而控制和管理进程。**PCB是进程存在的唯一标识**  
PCB包含以下信息：
1. **进程描述信息**：进程标识符，用户标识符
2. **进程控制管理信息**：进程当前状态，进程优先级
3. **进程资源分配清单**：内存地址空间、打开文件列表、所用IO设备信息
4. **CPU相关信息**：进程切换时CPU寄存器的值存在PCB中，以便重新执行

### 3. 并行与并发
1. **并发**是**单个**处理核在短时间执行多个进程。CPU切换进程必须在PCB中记录当前状态信息，一遍切换回来时继续执行
2. **并行**是**多个**处理核同时执行多个进程

### 4. 进程的状态切换
1. 运行态：进程占用CPU
2. 就绪态：可运行，等待其它运行中的进程
3. 阻塞态：等待IO等事件发生而暂停
4. 创建状态：进程正在被创建
5. 结束状态：进程正在从系统中消失
