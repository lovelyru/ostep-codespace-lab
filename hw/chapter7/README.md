# Q1
FIFO：
Total T_response = 600;
Total T_turnaround = 200+400+600 = 1200;
SJF:
Total T_response = 0+200+400 = 600;
Total T_turnaround = 200+400+600 = 1200
# Q2
FIFO:
Total T_response = 0+100+300 = 400;
Total T_turnaround = 100+300+600 = 1000;
SJF:
Total T_response = 0+100+300 = 400;
Total T_turnaround = 100+300+600 = 1000
# Q3
Total T_response = 3;
Total T_turnaround = 1397
# Q4
假设有3个job，分别是job0，job1，job2。只有在job0_length ≤ job1_length ≤ job2_length时，SJF和FIFO工作模式所需的Total T_turnaround才相等。
# Q5
假设有3个job，分别是job0，job1，job2。只有在job0_length = job1_length = job2_length，并且RR的时间片长度 q = job_length时，SJF和RR工作模式所需的Total T_response才相等。
# Q6
随着工作长度的增加，SJF的响应时间会增加。但在只有[最长的job的长度]发生改变，其余job的长度不变的情况下，响应时间不会变化(总工作长度增加)。
# Q7
随着 RR 时间片长度 q 增加，RR的响应时间会增加。
最坏情况的响应时间：T_response(MAX) = (N-1)*q

# PS
详细推导过程请看analysis.md文件。