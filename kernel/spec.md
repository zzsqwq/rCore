# 代码结构

## 驱动

按照分类放到 `src/drivers` 目录下。

如果是有 device tree 或者 pci 的平台，应当从对应的 bus 中初始化并传递参数。

如果是全局唯一并且代码不会复用的情况，可以写成 singleton。

## ISA 相关代码结构

路径：`src/arch/ISA`。其余的代码尽量不要出现平台相关的代码。

### consts

- KERNEL_OFFSET： 线性映射的偏移
- ARCH：ISA 的名称

### cpu

- fn id()：当前正在运行的 CPU 的 ID

### timer

- fn timer_now()：当前的时间

### interrupt

- fn ack(trap: usize)：确认中断处理
- fn timer()：处理时钟中断
- fn wait_for_interrupt()：打开并等待中断
- fn handle_user_page_fault(thread: &Arc\<Thread\>, addr: usize)：处理用户态的缺页异常

### interrupt/consts

- fn is_page_fault(trap: usize)：是否缺页
- fn is_syscall(trap: usize)：是否系统调用
- fn is_intr(trap: usize)：是否中断

### interrupt/handler

- fn trap_handler(tf: &mut TrapFrame)：处理来自内核的异常

### syscall

含有所有 syscall 的编号的定义

### board/xxx

针对该架构下某个平台的相关代码

- fn early_init()：早期初始化串口等
- fn early_final()：结束早期初始化串口
- fn init()：初始化

### mod

- fn main_start()：第一个核的启动
- fn others_start()：其他核的启动

### fp

- struct FpState：浮点的状态

### signal

- struct MachineContext：和 Linux 中的 mcontext 一致
- RETCODE：调用 rt_sigreturn syscall 的指令
- set_signal_handler：设置 signal handler
