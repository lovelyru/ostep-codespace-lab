## Chaper7
# Q1
- 题目:Compute the response time and turnaround time when running three jobs of length 200 with the SJF and FIFO schedulers.
- 预测:
```
USE -p FIFO 作业将按顺序执行，直到完成。
所以工作表大致如下：
work     T_firstrun   T_completion 
job0     0            200
joe1     200          400
job2     400          600

job0:
     T_response = 0 - 0 = 0;
     T_turnaround = 200 - 0 = 200
job1:
     T_response = 200 - 0 = 200;
     T_turnaround = 400 - 0 = 400
job2:
     T_response = 400 - 0 = 400;
     T_turnaround = 600 - 0 = 600

Total T_response = 0+200+400 = 600
Total T_turnaround = 200+400+600 = 1200

Avg T_response = 600/3 = 200
Avg T_turnaround = 1200/3 = 400

USE -p SJF 作业将按顺序执行，直到完成。
所以工作表大致如下：
work     T_firstrun   T_completion 
job0     0            200
joe1     200          400
job2     400          600

job0:
     T_response = 0 - 0 = 0;
     T_turnaround = 200 - 0 = 200
job1:
     T_response = 200 - 0 = 200;
     T_turnaround = 400 - 0 = 400
job2:
     T_response = 400 - 0 = 400;
     T_turnaround = 600 - 0 = 600

Total T_response = 0+200+400 = 600
Total T_turnaround = 200+400+600 = 1200

Avg T_response = 600/3 = 200
Avg T_turnaround = 1200/3 = 400
```
- 理由:
```
      使用了FIFO调度模式，三个Job依次执行 job0->job1->job2。由教材内容可得，T_arrival默认为0，由计算公式可得具体的T_response和T_turnaround。
      使用SJF调度模式，会优先执行长度最小的job，但由于题目中给定的三个job长度相等，所以运行结果和FIFO模式相同。
```
- 验证结果:
- 分析:

# Q2
- 题目:Now do the same but with jobs of different lengths: 100, 200, and 300.
- 预测:
```
USE -p FIFO 作业将按顺序执行，直到完成。
所以工作表大致如下：
work     T_firstrun   T_completion
job0     0            100
joe1     100          300
job2     300          600

job0:
     T_response = 0 - 0 = 0;
     T_turnaround = 100 - 0 = 100
job1:
     T_response = 100 - 0 = 100;
     T_turnaround = 300 - 0 = 300
job2:
     T_response = 300 - 0 = 300;
     T_turnaround = 600 - 0 = 600

Total T_response = 0+100+300 = 400
Total T_turnaround = 100+300+600 = 1000

Avg T_response = 400/3 = 133.33
Avg T_turnaround = 1000/3 = 333.33

USE -p SJF 作业将按顺序执行，直到完成。
所以工作表大致如下：
work     T_firstrun   T_completion
job0     0            100
joe1     100          300
job2     300          600

job0:
     T_response = 0 - 0 = 0;
     T_turnaround = 100 - 0 = 100
job1:
     T_response = 100 - 0 = 100;
     T_turnaround = 300 - 0 = 300
job2:
     T_response = 300 - 0 = 300;
     T_turnaround = 600 - 0 = 600

Total T_response = 0+100+300 = 400
Total T_turnaround = 100+300+600 = 1000

Avg T_response = 400/3 = 133.33
Avg T_turnaround = 1000/3 = 333.33
```
- 理由: 
```
   使用了FIFO调度模式，三个Job依次执行 job0->job1->job2。由教材内容可得，T_arrival默认为0，由计算公式可得具体的T_response和T_turnaround。
   使用SJF调度模式，会优先执行长度最小的job，但由于题目中给定的三个job长度是100，200，300，属于递增关系，所以运行结果和FIFO模式相同。

```
- 验证结果:
- 分析:

# Q3
- 题目:Now do the same, but also with the RR scheduler and a time-slice of 1.
- 预测:
```
USE -p RR -q 1 作业将按job0->job1->job2穿插执行，直到完成。
所以工作表大致如下：
work     T_firstrun   T_completion
job0     0            298
joe1     1            499
job2     2            600

job0:
     T_response = 0 - 0 = 0;
     T_turnaround = 298 - 0 = 298
job1:
     T_response = 1 - 0 = 1;
     T_turnaround = 499 - 0 = 499
job2:
     T_response = 2 - 0 = 2;
     T_turnaround = 600 - 0 = 600

Total T_response = 0+1+2 = 3
Total T_turnaround = 298+499+600 = 1397

Avg T_response = 3/3 = 1
Avg T_turnaround = 1397/3 ≈ 456.67
```
- 理由:
```
     RR的调用模式是让所有的job在进程中按先后顺序穿插执行，由题目要求得知，RR的时间戳 q=1。
     我把整个作业的运行过程分为3段，按时间排序为：
     1.job0，job1，job2三个作业穿插进行，直到job0完成；
     2.job1和job2穿插执行，直到job1完成；
     3.job2单独执行。
     第1阶段：
     job0,job1,job2从时间节点0开始执行，穿插执行99次，来到时间节点99*3=297。此时，job0还剩1次执行长度，job1还剩101次执行长度，job2还剩201次执行长度。在时间节点298时，job0执行，并执行完成。时间节点298-299时job1开始执行。进入下一阶段。
     第2阶段：
     job1，job2从时间节点298开始执行，穿插执行100次，来到时间节点299+100*2=498。此时，job1还剩1次执行长度，job2还剩101次执行长度。在时间节点499时，job1执行，并执行完成。时间节点499-500时job2开始执行。进入下一阶段。
     第3阶段：
     job2从时间节点499开始执行，并执行101次，直到执行完成，此时时间节点来到600。就此，程序全部执行完毕。
```
- 验证结果:
- 分析:

# Q4
- 题目:For what types of workloads does SJF deliver the same turnaround times as FIFO?
- 预测:
```
假设有3个job，分别是job0，job1，job2。只有在job0_length ≤ job1_length ≤ job2_length时，SJF和FIFO工作模式所需的Total T_turnaround才相等。
```
- 理由:
```
下面是验证表：
情况1:假设job0_length = 100；job1_length = 200；job2_length = 300。
FIFO方法：
work     T_firstrun   T_completion
job0     0            100
joe1     100          300
job2     300          600
Total T_turnaround = 100+300+600 = 1000

SJF方法：
work     T_firstrun   T_completion
job0     0            100
joe1     100          300
job2     300          600
Total T_turnaround* = 100+300+600 = 1000

Total T_turnaround = Total T_turnaround*
PS:不再列举相等情况，因为结果是一致的。


情况2:假设job0_length = 300；job1_length =100；job2_length=200。
FIFO方法：
work     T_firstrun   T_completion
job0     0            300
joe1     300          400
job2     400          600
Total T_turnaround = 300+400+600 = 1300

SJF方法：
work     T_firstrun   T_completion
job1     0            100
joe2     100          300
job0     300          600
Total T_turnaround* = 100+300+600 = 1000

Total T_turnaround ≠ Total T_turnaround*

所以，只有在job0_length ≤ job1_length ≤ job2_length时，SJF和FIFO工作模式所需的Total T_turnaround才相等。
```
- 验证结果:
- 分析:

# Q5
- 题目:For what types of workloads and quantum lengths does SJF deliver the same response times as RR?
- 预测:
```
假设有3个job，分别是job0，job1，job2。只有在job0_length = job1_length = job2_length，并且RR的时间片长度 q = job_length时，SJF和RR工作模式所需的Total T_response才相等。
```
- 理由:
```
下面是验证表：
情况1:假设job0_length = 100；job1_length = 100；job2_length = 100；RR q = 100。
SJF方法：
work     T_firstrun   T_completion
job0     0            100
joe1     100          200
job2     200          300

Total T_response = 0+100+200 = 300

RR方法，RR q = 100：
work     T_firstrun   T_completion
job0     0            100
joe1     100          200
job2     200          300

Total T_response* = 0+100+200 = 300

Total T_response = Total T_response*


情况2:假设job0_length = 100；job1_length = 100；job2_length = 100；RR q = 1。
SJF方法：
work     T_firstrun   T_completion
job0     0            100
joe1     100          200
job2     200          300
Total T_response = 0+100+200 = 300

RR方法，RR q = 100：
work     T_firstrun   T_completion
job0     0            298
joe1     1            299
job2     2            300
Total T_response* = 0+1+2 = 3

Total T_response ≠ Total T_response*

所以只有在job0_length = job1_length = job2_length，并且RR的时间片长度 q = job_length时，SJF和RR工作模式所需的Total T_response才相等。
```
- 验证结果:
- 分析:

# Q6
- 题目:What happens to response time with SJF as job lengths increase?Can you use the simulator to demonstrate the trend?
- 预测:
```
随着工作长度的增加，SJF的响应时间会增加。但在只有[最长的job的长度]发生改变，其余job的长度不变的情况下，响应时间不会变化(总工作长度增加)。
```
- 理由:
```
假设有3个job同时到达。

情况1:假设job0_length = 100；job1_length = 200；job2_length = 300。
验证表如下：
work     T_firstrun   T_completion
job0     0            100
joe1     100          200
job2     200          300

Total T_response = 0+100+200 = 300

情况2:假设job0_length = 200；job1_length = 300；job2_length = 400。
验证表如下：
work     T_firstrun   T_completion
job0     0            200
joe1     200          500
job2     500          900

Total T_response = 0+200+500 = 700


情况3:在单个[最长的job的长度]变化，其余job长度不变时。
α：job0_length = 100；job1_length = 200；job2_length = 300。
验证表如下：
work     T_firstrun   T_completion
job0     0            100
joe1     100          200
job2     200          300

Total T_response = 0+100+200 = 300

β：此时改变最长的job长度，即改变job2_length。job0_length = 100；job1_length = 200；job2_length = 700。
验证表如下：
work     T_firstrun   T_completion
job0     0            100
joe1     100          200
job2     200          900

Total T_response* = 0+100+200 = 300

可以看到Total T_response = Total T_response*


所以，在SJF模式中，因为任务是按照job的长短顺序依次执行的。job的长度增加时，排在后面的job需要等待更长时间，响应时间也会增加。只是增加原有最长的一个job长度，总响应时间不改变。
```
- 验证结果:
- 分析:

# Q7
- 题目:What happens to response time with RR as quantum lengths increase? Can you write an equation that gives the worst-case response time, given N jobs?
- 预测:
```
随着 RR 时间片长度 q 增加，RR的响应时间会增加。
最坏情况的响应时间：T_response(MAX) = (N-1)*q
```
- 理由:
```
情况1:假设四个job同时到达，job0_length = 10；job1_length = 20；job2_length = 30；job3_length = 40；RR q = 1。
验证表如下：
work     T_firstrun   T_completion
job0     0            37           4x9+1   11,21,31
joe1     1            68           3x10+1  10,20
job2     2            87           2x9+1   11
job3     3            98

Total T_response = 0+1+2+3 = 6

情况2:假设三个job同时到达，job0_length = 10；job1_length = 20；job2_length = 30；job3_length = 40；RR q = 2。
验证表如下：
work     T_firstrun   T_completion
job0     0            
joe1     2            
job2     4     
job3     6        

Total T_response = 0+2+4+6 = 12

情况3:假设三个job同时到达，job0_length = 10；job1_length = 20；job2_length = 30；job3_length = 40；RR q = 3。
验证表如下：
work     T_firstrun   T_completion
job0     0            
joe1     3            
job2     6  
job3     9          

Total T_response = 0+3+6+9 = 18

情况4:假设三个job同时到达，job0_length = 10；job1_length = 20；job2_length = 30；job3_length = 40；RR q = 4。
验证表如下：
work     T_firstrun   T_completion
job0     0            
joe1     4            
job2     8
job3     12            

Total T_response = 0+4+8+12 = 24

由以上结果规律推导：
所有job从0开始计数。
单个job的响应时间：T_response(i) = i*q
最坏情况的响应时间：T_response(MAX) = (N-1)*q
```
- 验证结果:
- 分析: