## Q1
- Prediction / 预测:总时间为 10 ticks，CPU 利用率为 100.00%。
- Reasoning / 理由:两个进程各包含 5 条 CPU 指令（无 I/O）。PID 0 先运行占用 CPU 5 个 tick，期间 PID 1 处于 READY 状态；PID 0 完成后，PID 1 占用 CPU 运行 5 个 tick，因此 CPU 全程无空闲。
- Verified result / 验证结果:
- Analysis / 分析:


## Q2
- Prediction / 预测:总时间为 11，CPU 利用率为 54.55%，I/O 利用率为 45.45%。
- Reasoning / 理由:-l 4:100,1:0 意味着 PID 0 需要 4 个 CPU tick（不做 I/O），PID 1 需要 1 个 CPU tick，然后发起 I/O（默认耗时 5 个 tick）。PID 0 先运行 4 个 tick，PID 1 运行 1 个 tick 后进入 BLOCKED 等待 I/O。此时 CPU 空闲，PID 1 等待 5 个 tick。I/O 完成后，PID 1 再运行最后 1 个 CPU tick 完成。
- Verified result / 验证结果:
- Analysis / 分析: 

## Q3
- Prediction / 预测:总时间为 7，CPU 利用率为 85.71%，I/O 利用率为 71.43%。
- Reasoning / 理由:-l 1:0,4:100 交换了顺序。PID 0 先运行 1 个 tick，然后发起 I/O（进入 BLOCKED，耗时 5 个 tick）。此时 CPU 空闲，调度 PID 1 运行 4 个 tick。在 PID 1 运行期间，PID 0 的 I/O 也在同步进行。PID 0 的 I/O 完成后，再运行 1 个 CPU tick。总时间被重叠的 CPU 和 I/O 大大缩短。
- Verified result / 验证结果:
- Analysis / 分析:
## Q4
- Prediction / 预测:总时间为 11，CPU 利用率为 54.55%，I/O 利用率为 45.45%。
- Reasoning / 理由:命令加了 -S SWITCH_ON_END。这意味着当进程发起 I/O 时，操作系统不会切换到另一个进程，直到当前进程完全结束。因此，PID 0 发起 I/O 后，OS 拒绝调度 PID 1，导致 CPU 在整个 I/O 期间（5个tick）完全空转，白白浪费。
- Verified result / 验证结果:
- Analysis / 分析:

## Q5
- Prediction / 预测:总时间为 7，CPU 利用率为 85.71%，I/O 利用率为 71.43%。
- Reasoning / 理由:命令加了 -S SWITCH_ON_IO。这是模拟器的标准/默认行为。当进程发起 I/O 时，OS 立即切换到另一个就绪进程（PID 1）。PID 1 运行 4 个 tick，此时 PID 0 的 I/O 正在并行进行。总时间得到最大化压缩。
- Verified result / 验证结果:
- Analysis / 分析：

## Q6
- Prediction / 预测:总时间为 7，CPU 利用率为 85.71%，I/O 利用率为 71.43%。
- Reasoning / 理由: 命令加了 -I IO_RUN_LATER。这意味着当 I/O 完成时，完成的进程（PID 0）不会立即抢占当前正在运行的进程（PID 1），而是进入 READY 队列等待。由于 PID 1 只需要 4 个 tick，它在 PID 0 的 I/O（5个tick）完成之前就已经结束了，所以 CPU 在 Tick 6 是空闲的，PID 0 完成后直接运行。
- Verified result / 验证结果:
- Analysis / 分析: 

## Q7
- Prediction / 预测:总时间为 7，CPU 利用率为 85.71%，I/O 利用率为 71.43%。
- Reasoning / 理由:命令加了 -I IO_RUN_IMMEDIATE（你之前跑出 Total Time 21 就是这个参数，但那是因为进程数不同）。当 PID 0 的 I/O 完成时，它立即抢占 CPU 运行。在我们的场景下，PID 1 已经完成了，CPU 空闲，所以 PID 0 立即运行。
- Verified result / 验证结果:

- Analysis / 分析: 

## Q8
- Prediction / 预测:随机种子 `-s 1`, `-s 2`, `-s 3` 会随机生成进程的 CPU 指令和 I/O 指令序列。不同参数设置对运行结果的影响如下：
1. 默认设置（`-S SWITCH_ON_IO -I IO_RUN_LATER`）：当进程发起 I/O 时会切换 CPU 给其他进程，但 I/O 完成后不会立刻抢占当前正在运行的进程，整体重叠利用率较高。
2. 立即运行设置（`-I IO_RUN_IMMEDIATE`）：I/O 完成后立刻抢占 CPU，能让 I/O 密集型进程更快发起下一次 I/O，从而提高整体 I/O 与 CPU 的并行度，减少总运行时间。
3. 顺序运行设置（`-S SWITCH_ON_END`）：不进行 I/O 切换，进程必须完全运行结束才换下一个，会导致 CPU 在 I/O 期间大量空闲，总运行时间最长，效率最低。
- Reasoning / 理由:不同的抢占与切换策略决定了 CPU 资源在等待 I/O 期间能否被充分利用。`-S SWITCH_ON_IO` 与 `-I IO_RUN_IMMEDIATE` 组合能最大程度发挥 CPU 和 I/O 设备并行工作的能力。
- Verified result / 验证结果:
- Analysis / 分析:

