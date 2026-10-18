# Chapter 7: CPU Scheduling - Analysis Report

## Q1: SJF and FIFO with 3 jobs of length 200
- **FIFO / SJF**: 3个作业长度相同（均为200）且同时到达，因此FIFO和SJF的调度顺序和结果完全相同。
- **Average Response Time**: 200.00
- **Average Turnaround Time**: 400.00

## Q2: SJF and FIFO with jobs of length 100, 200, 300
- **FIFO**: 平均响应时间为 133.33，平均周转时间为 333.33。
- **SJF**: 平均响应时间为 66.67，平均周转时间为 233.33。SJF优先运行短作业，显著降低了平均周转时间。

## Q3: Round-Robin (RR) with quantum = 1
- **RR (-q 1)**: 平均响应时间极低（0.67），但平均周转时间非常高（566.67）。体现了时间片轮转对交互式响应友好的特点，但频繁切换导致周转时间恶化。

## Q4: When does SJF provide the same turnaround time as FIFO?
- 当所有作业的运行时间完全相同，或者作业到达时已经按照运行时间从小到大（非递减顺序）排列时，SJF 和 FIFO 的周转时间相同。

## Q5: When do SJF and RR provide the same response time?
- 当所有作业的运行长度小于或等于时间片（Quantum），且同时到达时，RR 与 SJF 的首次运行表现相同。

## Q6: How does SJF response time change as job length increases?
- 在非抢占式SJF中，排在后面的作业必须等待前面的作业全部完成。随着前面作业长度的增加，后续作业的响应时间会线性/累积增加。

## Q7: How does RR response time change as quantum increases, and worst-case formula?
- **趋势**：随着时间片（Quantum）增加，RR 逐渐退化为 FIFO，后到达或排在后面的作业等待时间变长，平均响应时间变差。
- **最坏情况响应时间公式**（对于 $N$ 个同时到达的作业，时间片为 $Q$）：
  $$T_{response, worst} = (N - 1) \times Q$$
