# OSTEP Ch.4 Homework Answers
## Q1
- Prediction / 预测:
```
Time   PID:0   PID:1   CPU
1      RUN     READY    1
2      RUN     READY    1
3      RUN     READY    1
4      RUN     READY    1
5      RUN     READY    1
6      DONE    RUN      1
7      DONE    RUN      1
8      DONE    RUN      1
9      DONE    RUN      1
10     DONE    RUN      1
Total Time = 10
IO Busy = 0 ( 0% )
CPU Busy = 10 ( 100% )
```
- Think：
        两个进程都只用CPU时，不会出现空闲。
- Reasoning / 理由:
        Process 0 先占用并使用 5 次 CPU，执行完成后，Process 1 再使用 5 次。由终端输出可知，该模块使用了 10 条 tick —— Total time = 10，其中 I/O 没有进行操作，所以 CPU利用率 = 100%，I/O利用率 = 0%。
- Verified result / 验证结果:
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2        RUN:cpu         READY             1          
  3        RUN:cpu         READY             1          
  4        RUN:cpu         READY             1          
  5        RUN:cpu         READY             1          
  6           DONE       RUN:cpu             1          
  7           DONE       RUN:cpu             1          
  8           DONE       RUN:cpu             1          
  9           DONE       RUN:cpu             1          
 10           DONE       RUN:cpu             1          

Stats: Total Time 10
Stats: CPU Busy 10 (100.00%)
Stats: IO Busy  0 (0.00%)
```
- Analysis / 分析:预测正确
## Q2
- Prediction / 预测:
```
Time   PID:0   PID:1        CPU   IOs
1      RUN     READY        1
2      RUN     READY        1
3      RUN     READY        1
4      RUN     READY        1
5      DONE    RUN:io       1
6      DONE    BLOCKED            1
7      DONE    BLOCKED            1
8      DONE    BLOCKED            1
9      DONE    BLOCKED            1
10     DONE    BLOCKED            1
11*    DONE    RUN:io_done  1
Total Time = 11
CPU Busy = 6 ( 54.55% )
IO Busy = 5 ( 45.45% )
```
- Think：
        PID 0结束前PID 1没有运行，因为CPU在被占用，PID 1此时等待运行。一次I/O需要7个tick，分别为1:RUN:io,2~6:default，7:RUN:io_done,但在这个过程中，I/O只有在time2~6的时候会繁忙，因为io和io_done是CPU执行的。
- Reasoning / 理由:
        Process 0 首先在100%使用CPU的情况下执行了4条CPU命令，然后切换到Process 1。Process 1先执行I/O，系统立刻被切换，但Process 0 DONE，所以Process 1进入BLOCKED，并在默认条件下等待5个ticks，在I/O执行完毕后，Process 1再利用一个tick执行io_done。所以Total time = 11，又已知PID:0执行了4条ticks，PID:1利用CPU执行了一次io和一次io_done所以CPU Busy=6，所以CPU占用率是6/11≈54.55%,I/O占用率≈45.45%。
- Verified result / 验证结果:
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2        RUN:cpu         READY             1          
  3        RUN:cpu         READY             1          
  4        RUN:cpu         READY             1          
  5           DONE        RUN:io             1          
  6           DONE       BLOCKED                           1
  7           DONE       BLOCKED                           1
  8           DONE       BLOCKED                           1
  9           DONE       BLOCKED                           1
 10           DONE       BLOCKED                           1
 11*          DONE   RUN:io_done             1          

Stats: Total Time 11
Stats: CPU Busy 6 (54.55%)
Stats: IO Busy  5 (45.45%)
```
- Analysis / 分析:预测正确
## Q3
- Prediction / 预测:
```
Time  PID 0        PID 1   CPU    IOs
1     RUN:io       READY   1
2     BLOCKED      RUN     1      1
3     BLOCKED      RUN     1      1
4     BLOCKED      RUN     1      1
5     BLOCKED      RUN     1      1
6     BLOCKED      DONE           1
7*    RUN:io_done  DONE    1
Total Time = 7
CPU Busy = 6 ( 85.71% )
IO Busy = 5 ( 71.43% )
```
- Think：
        由于PID 0首先在time1执行RUN:io，被I/O阻塞后，可以转移给另一个进程。只看PID 0，time2~6CPU处于空闲状态，所以PID 1可以CPU利用执行命令，所以在Q3中，I/O被阻塞时，CPU在执行PID 1的命令。
- Reasoning / 理由:
        Process 1先利用CPU执行RUN:io,然后开始运行I/O设备，由于I/O设备运行当中CPU为空闲状态，Process 0在time=2的状态下应该是BLOCKED，并等待5个ticks。所以Process 1可以运行CPU，故切换到Process 1开始执行，直到time=4时执行完成，CPU进入完成状态，并继续等待1个tick。在等待完成后执行RUN:io_done。所以Total time=7，CPU占用了6个ticks，同时IO占用了5个ticks，CPU占用率为6/7≈85.71%，I/O占用率为5/7≈71.43%。
- Verified result / 验证结果:
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1          

Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
```
- Analysis / 分析:预测正确
## Q4
- Prediction / 预测:
```
Time  PID 0        PID 1   CPU    IOs
1     RUN:io       READY   1
2     BLOCKED      READY          1
3     BLOCKED      READY          1
4     BLOCKED      READY          1
5     BLOCKED      READY          1
6     BLOCKED      READY          1
7*    RUN:io_done  READY   1
8     DONE         RUN     1
9     DONE         RUN     1
10    DONE         RUN     1
11    DONE         RUN     1
Total Time = 11
CPU Busy = 6 ( 54.55% )
IO Busy = 5 （ 45.45% ）
```
- Think：
        I/O等待时不切换CPU被占用，状态为READY。
- Reasoning / 理由:
        由终端输出可知：System will switch when the current process is FINISHED After IOs, the process issuing the IO will run LATER (when it is its turn).所以PID 0需要先把I/O阻断执行完成，再转移到PID 1执行CPU命令。所以time1~7是PID 0执行RUN:io，等待5个ticks，再执行RUN:io_done，执行完毕后，进程转到PID 1，执行4个RUN:CPU。所以Total time = 11。CPU占用了6个ticks，CPU占用率是6/11≈54.55%。
- Verified result / 验证结果:
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*   RUN:io_done         READY             1          
  8           DONE       RUN:cpu             1          
  9           DONE       RUN:cpu             1          
 10           DONE       RUN:cpu             1          
 11           DONE       RUN:cpu             1          

Stats: Total Time 11
Stats: CPU Busy 6 (54.55%)
Stats: IO Busy  5 (45.45%)
- Analysis / 分析:预测正确
## Q5
- Prediction / 预测:
```
Time  PID 0        PID 1   CPU  IOs
1     RUN:io       READY   1    
2     BLOCKED      RUN     1    1
3     BLOCKED      RUN     1    1
4     BLOCKED      RUN     1    1
5     BLOCKED      RUN     1    1
6     BLOCKED      DONE         1
7*    RUN:io_done  DONE    1
Total Time = 7
CPU Busy = 6 ( 85.71% )
IO Busy = 5 ( 71.43% )
```
- Think：
        I/O阻断后不占用CPU，在PID 0 RUN:io后立刻转到PID 1运行CPU。因为I/O和CPU同时运行，所以总时间缩短。
- Reasoning / 理由:
        和Q3输出结果相同，但对比Q4，使用SWITCH_ON_IO命令，不强制I/O阻塞，即启动I/O时CPU可切换。所以PID 0在time1时启动I/O，不强制占用CPU，随后PID 1可执行CPU命令，所以time2~5PID 1执行RUN:CPU。直到PID 0运行RUN:io_done，进程结束。Total time、CPU占用、I/O占用和Q3一致。
- Verified result / 验证结果:
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1          

Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
```
- Analysis / 分析:预测正确
## Q6
- Prediction / 预测:
```
Time  PID 0        PID 1   PID 2   PID 3   CPU  IOs
1     RUN:io       READY   READY   READY   1
2     BLOCKED      RUN     READY   READY   1    1
3     BLOCKED      RUN     READY   READY   1    1
4     BLOCKED      RUN     READY   READY   1    1
5     BLOCKED      RUN     READY   READY   1    1
6     BLOCKED      RUN     READY   READY   1    1
7*    RUN:io_done  DONE    READY   READY   1
8     RUN:io       DONE    READY   READY   1
9     BLOCKED      DONE    RUN     READY   1    1
10    BLOCKED      DONE    RUN     READY   1    1
11    BLOCKED      DONE    RUN     READY   1    1
12    BLOCKED      DONE    RUN     READY   1    1
13    BLOCKED      DONE    RUN     READY   1    1
14*   RUN:io_done  DONE    DONE    READY   1
15    RUN:io       DONE    DONE    READY   1
16    BLOCKED      DONE    DONE    RUN     1    1
17    BLOCKED      DONE    DONE    RUN     1    1
18    BLOCKED      DONE    DONE    RUN     1    1
19    BLOCKED      DONE    DONE    RUN     1    1
20    BLOCKED      DONE    DONE    RUN     1    1
21*   RUN:io_done  DONE    DONE    DONE    1
Total Time = 21
CPU Busy = 21( 100% )
IO Busy = 15( 71.43% )
```
- Think：
        PID 0RUN:io后由于使用了IO_RUN_LATER指令，所以需要等待5次默认ticks后执行RUN:io_done,期间IOs不空闲。
- Reasoning / 理由:
        由于使用了SWITCH_ON_IO和IO_RUN_LATER指令，所以IO阻塞不占用CPU，并且IO在CPU执行完成前处于等待状态，但默认的IO阻塞时间是5个ticks，指令输出中，P1,P2,P3使用的CPU也是各五个，所以输出结果和
        ```
        python3 process-run.py -l 3:0,5:100,5:100,5:100理应相同。
        ```
        所以Total time=21，CPU Buys=21，占用率为100%。而IO在进程中使用了15个ticks，所以IO Buys=15，占用率为15/21≈71.43%。
- Verified result / 验证结果:
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1          
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7*         READY          DONE       RUN:cpu         READY             1          
  8          READY          DONE       RUN:cpu         READY             1          
  9          READY          DONE       RUN:cpu         READY             1          
 10          READY          DONE       RUN:cpu         READY             1          
 11          READY          DONE       RUN:cpu         READY             1          
 12          READY          DONE          DONE       RUN:cpu             1          
 13          READY          DONE          DONE       RUN:cpu             1          
 14          READY          DONE          DONE       RUN:cpu             1          
 15          READY          DONE          DONE       RUN:cpu             1          
 16          READY          DONE          DONE       RUN:cpu             1          
 17    RUN:io_done          DONE          DONE          DONE             1          
 18         RUN:io          DONE          DONE          DONE             1          
 19        BLOCKED          DONE          DONE          DONE                           1
 20        BLOCKED          DONE          DONE          DONE                           1
 21        BLOCKED          DONE          DONE          DONE                           1
 22        BLOCKED          DONE          DONE          DONE                           1
 23        BLOCKED          DONE          DONE          DONE                           1
 24*   RUN:io_done          DONE          DONE          DONE             1          
 25         RUN:io          DONE          DONE          DONE             1          
 26        BLOCKED          DONE          DONE          DONE                           1
 27        BLOCKED          DONE          DONE          DONE                           1
 28        BLOCKED          DONE          DONE          DONE                           1
 29        BLOCKED          DONE          DONE          DONE                           1
 30        BLOCKED          DONE          DONE          DONE                           1
 31*   RUN:io_done          DONE          DONE          DONE             1          

Stats: Total Time 31
Stats: CPU Busy 21 (67.74%)
Stats: IO Busy  15 (48.39%)
```
- Analysis / 分析:
## Q7
- Prediction / 预测:
```
Time  PID 0        PID 1   PID 2   PID 3   CPU  IOs
1     RUN:io       READY   READY   READY   1
2     BLOCKED      RUN     READY   READY   1    1
3     BLOCKED      RUN     READY   READY   1    1
4     BLOCKED      RUN     READY   READY   1    1
5     BLOCKED      RUN     READY   READY   1    1
6     BLOCKED      RUN     READY   READY   1    1
7*    RUN:io_done  DONE    READY   READY   1
8     RUN:io       DONE    READY   READY   1
9     BLOCKED      DONE    RUN     READY   1    1
10    BLOCKED      DONE    RUN     READY   1    1
11    BLOCKED      DONE    RUN     READY   1    1
12    BLOCKED      DONE    RUN     READY   1    1
13    BLOCKED      DONE    RUN     READY   1    1
14*   RUN:io_done  DONE    DONE    READY   1
15    RUN:io       DONE    DONE    READY   1
16    BLOCKED      DONE    DONE    RUN     1    1
17    BLOCKED      DONE    DONE    RUN     1    1
18    BLOCKED      DONE    DONE    RUN     1    1
19    BLOCKED      DONE    DONE    RUN     1    1
20    BLOCKED      DONE    DONE    RUN     1    1
21*   RUN:io_done  DONE    DONE    DONE    1
Total Time = 21
CPU Busy = 21( 100% )
IO Busy = 15( 71.43% )
```
- Think：
        节省总时间。
- Reasoning / 理由:
        由于使用了IO_RUN_IMMEDIATE，所以在IO完成后CPU马上继续运行，并且SWITCH_ON_IO不占用CPU进程。但由于Q6中的实例特殊，所以Q7和Q6的输出结果应该一致，如果Q6中的命令是
        ```
        python3 ··· -l 3:0,4:100,4:100,4:100 -S SWITCH_ON_IO -I IO_RUN_LATER
        ```
        那么在Q6中，CPU Buys=18，因为P1,P2,P3各运行了4次CPU。
- Verified result / 验证结果:
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1          
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7*   RUN:io_done          DONE         READY         READY             1          
  8         RUN:io          DONE         READY         READY             1          
  9        BLOCKED          DONE       RUN:cpu         READY             1             1
 10        BLOCKED          DONE       RUN:cpu         READY             1             1
 11        BLOCKED          DONE       RUN:cpu         READY             1             1
 12        BLOCKED          DONE       RUN:cpu         READY             1             1
 13        BLOCKED          DONE       RUN:cpu         READY             1             1
 14*   RUN:io_done          DONE          DONE         READY             1          
 15         RUN:io          DONE          DONE         READY             1          
 16        BLOCKED          DONE          DONE       RUN:cpu             1             1
 17        BLOCKED          DONE          DONE       RUN:cpu             1             1
 18        BLOCKED          DONE          DONE       RUN:cpu             1             1
 19        BLOCKED          DONE          DONE       RUN:cpu             1             1
 20        BLOCKED          DONE          DONE       RUN:cpu             1             1
 21*   RUN:io_done          DONE          DONE          DONE             1          

Stats: Total Time 21
Stats: CPU Busy 21 (100.00%)
Stats: IO Busy  15 (71.43%)

```
- Analysis / 分析:预测正确
## Q8
- Prediction / 预测:
# Seed 1
```
Time  PID 0        PID 1   CPU  IOs
1     RUN          READY   1    
2     RUN:io       READY   1    
3     BLOCKED      RUN     1    1
4     BLOCKED      RUN     1    1
5     BLOCKED      RUN     1    1
6     BLOCKED      DONE         1
7     BLOCKED      DONE         1
8*    RUN:io_done  DONE    1
9     RUN:io       DONE    1    
10    BLOCKED      DONE         1
11    BLOCKED      DONE         1
12    BLOCKED      DONE         1
13    BLOCKED      DONE         1
14    BLOCKED      DONE         1
15*   RUN:io_done  DONE    1
Total Time = 15
CPU Busy = 8 ( 53.33% )
IO Busy = 10 ( 66.67% )
```
- Reasoning / 理由:
Seed1中，PID 0先运行CPU，随即执行I/O阻塞，但在I/O过程中PID 1可以执行RUN:cpu,在time3~5 PID 1执行RUN:cpu，后PID 1不再执行任何程序，所以后续皆为DONE的状态，PID 0则依次执行I/O阻塞。
- Verified result / 验证结果:
```
### default
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7        BLOCKED          DONE                           1
  8*   RUN:io_done          DONE             1          
  9         RUN:io          DONE             1          
 10        BLOCKED          DONE                           1
 11        BLOCKED          DONE                           1
 12        BLOCKED          DONE                           1
 13        BLOCKED          DONE                           1
 14        BLOCKED          DONE                           1
 15*   RUN:io_done          DONE             1          

Stats: Total Time 15
Stats: CPU Busy 8 (53.33%)
Stats: IO Busy  10 (66.67%)

### -I IO_RUN_IMMEDIATE
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7        BLOCKED          DONE                           1
  8*   RUN:io_done          DONE             1          
  9         RUN:io          DONE             1          
 10        BLOCKED          DONE                           1
 11        BLOCKED          DONE                           1
 12        BLOCKED          DONE                           1
 13        BLOCKED          DONE                           1
 14        BLOCKED          DONE                           1
 15*   RUN:io_done          DONE             1          

Stats: Total Time 15
Stats: CPU Busy 8 (53.33%)
Stats: IO Busy  10 (66.67%)

### -S SWITCH_ON_END
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7        BLOCKED         READY                           1
  8*   RUN:io_done         READY             1          
  9         RUN:io         READY             1          
 10        BLOCKED         READY                           1
 11        BLOCKED         READY                           1
 12        BLOCKED         READY                           1
 13        BLOCKED         READY                           1
 14        BLOCKED         READY                           1
 15*   RUN:io_done         READY             1          
 16           DONE       RUN:cpu             1          
 17           DONE       RUN:cpu             1          
 18           DONE       RUN:cpu             1          

Stats: Total Time 18
Stats: CPU Busy 8 (44.44%)
Stats: IO Busy  10 (55.56%)

```
- Analysis / 分析:default预测正确。但没有使用 -I 和 -S命令行。在使用 -I IO_RUN_IMMEDIATE命令行时，由于进行完RUN:io后直接运行另一个PID的RUN:cpu指令，所以和default的输出结果一致。在使用 -S SWITCH_ON_END命令行时，由于需要把PID 0的I/O阻塞全部运行完成才可以运行PID 1的RUN：cpu命令，所以Total time = 18。CPU Busy、IO Busy不变，CPU占用率为8/18≈44.44%，IO占用率为10/18≈55.56%。
# Seed 2
```
Time  PID 0        PID 1         CPU  IOs
1     RUN:io       READY         1    
2     BLOCKED      RUN           1    1
3     BLOCKED      RUN:io        1    1
4     BLOCKED      BLOCKED            1
5     BLOCKED      BLOCKED            1
6     BLOCKED      BLOCKED            1
7*    RUN:io_done  BLOCKED       1    1
8     RUN:io       BLOCKED       1    1
9     BLOCKED      RUN:io_done   1    1
10*   BLOCKED      RUN:io        1    1
11    BLOCKED      BLOCKED            1
12    BLOCKED      BLOCKED            1
13    BLOCKED      BLOCKED            1
14*   RUN:io_done  BLOCKED       1    1
15    RUN          BLOCKED            1
16*   DONE         RUN:io_done   1
Total Time = 16
CPU Busy = 9 ( 56.25% )
IO Busy = 14 ( 87.50% )
```
- Reasoning / 理由:
Seed 2中，PID 0依次执行2次I/O阻塞，和1次RUN:cpu,执行完毕后状态变为DONE。PID 1依次执行1次RUN:cpu，2次I/O阻塞。
- Verified result / 验证结果:
```
### default
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7*   RUN:io_done       BLOCKED             1             1
  8         RUN:io       BLOCKED             1             1
  9*       BLOCKED   RUN:io_done             1             1
 10        BLOCKED        RUN:io             1             1
 11        BLOCKED       BLOCKED                           2
 12        BLOCKED       BLOCKED                           2
 13        BLOCKED       BLOCKED                           2
 14*   RUN:io_done       BLOCKED             1             1
 15        RUN:cpu       BLOCKED             1             1
 16*          DONE   RUN:io_done             1          

Stats: Total Time 16
Stats: CPU Busy 10 (62.50%)
Stats: IO Busy  14 (87.50%)

### -I IO_RUN_IMMEDIATE
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7*   RUN:io_done       BLOCKED             1             1
  8         RUN:io       BLOCKED             1             1
  9*       BLOCKED   RUN:io_done             1             1
 10        BLOCKED        RUN:io             1             1
 11        BLOCKED       BLOCKED                           2
 12        BLOCKED       BLOCKED                           2
 13        BLOCKED       BLOCKED                           2
 14*   RUN:io_done       BLOCKED             1             1
 15        RUN:cpu       BLOCKED             1             1
 16*          DONE   RUN:io_done             1          

Stats: Total Time 16
Stats: CPU Busy 10 (62.50%)
Stats: IO Busy  14 (87.50%)

### -S SWITCH_ON_END
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*   RUN:io_done         READY             1          
  8         RUN:io         READY             1          
  9        BLOCKED         READY                           1
 10        BLOCKED         READY                           1
 11        BLOCKED         READY                           1
 12        BLOCKED         READY                           1
 13        BLOCKED         READY                           1
 14*   RUN:io_done         READY             1          
 15        RUN:cpu         READY             1          
 16           DONE       RUN:cpu             1          
 17           DONE        RUN:io             1          
 18           DONE       BLOCKED                           1
 19           DONE       BLOCKED                           1
 20           DONE       BLOCKED                           1
 21           DONE       BLOCKED                           1
 22           DONE       BLOCKED                           1
 23*          DONE   RUN:io_done             1          
 24           DONE        RUN:io             1          
 25           DONE       BLOCKED                           1
 26           DONE       BLOCKED                           1
 27           DONE       BLOCKED                           1
 28           DONE       BLOCKED                           1
 29           DONE       BLOCKED                           1
 30*          DONE   RUN:io_done             1          

Stats: Total Time 30
Stats: CPU Busy 10 (33.33%)
Stats: IO Busy  20 (66.67%)

```
- Analysis / 分析:default中在time15时RUN:cpu命令被我漏掉了，所以少了一个CPU进程。没有使用 -I 和 -S命令行。在使用 -I IO_RUN_IMMEDIATE命令行时，由于进行完RUN:io后直接运行另一个PID的RUN:cpu指令，所以和default的输出结果一致。在使用 -S SWITCH_ON_END命令行时由于需要在PID 0和PID 1各自的的管道内的I/O阻塞全部运行完成才可以运行各自的RUN：cpu命令，所以Total time = 30。CPU Busy=10,IO Busy=20.CPU占用率为10/30≈33.33%，IO占用率为20/30≈66.67%。
# Seed 3
```
Time  PID 0        PID 1         CPU  IOs
1     RUN          READY         1    
2     RUN:io       READY         1    1
3     BLOCKED      RUN:io        1    1
4     BLOCKED      BLOCKED            1
5     BLOCKED      BLOCKED            1
6     BLOCKED      BLOCKED            1
7*    BLOCKED      BLOCKED            1
8     RUN:io_done  BLOCKED       1    1
9     READY        RUN:io_done   1    1
10*   READY        RUN:io        1    1
11    RUN          BLOCKED       1    1
12    DONE         BLOCKED            1
13    DONE         BLOCKED            1
14*   DONE         BLOCKED            1
15    DONE         BLOCKED            1
16*   DONE         RUN:io_done   1
17    DONE         RUN           1
Total Time = 17
CPU Busy = 9 ( 52.94% )
IO Busy = 14 ( 82.35% )
```
- Reasoning / 理由:
Seed3中，PID 0依次执行1次RUN:cpu，1次I/O阻塞，1次RUN:cpu，执行完毕后在time12时状态变为DONE，PID 1先等待2个ticks，然后依次执行2次I/O阻塞，1次RUN:cpu。
- Verified result / 验证结果:
```
### default
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7        BLOCKED       BLOCKED                           2
  8*   RUN:io_done       BLOCKED             1             1
  9*       RUN:cpu         READY             1          
 10           DONE   RUN:io_done             1          
 11           DONE        RUN:io             1          
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16           DONE       BLOCKED                           1
 17*          DONE   RUN:io_done             1          
 18           DONE       RUN:cpu             1          

Stats: Total Time 18
Stats: CPU Busy 9 (50.00%)
Stats: IO Busy  11 (61.11%)

### -I IO_RUN_IMMEDIATE
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7        BLOCKED       BLOCKED                           2
  8*   RUN:io_done       BLOCKED             1             1
  9*         READY   RUN:io_done             1          
 10          READY        RUN:io             1          
 11        RUN:cpu       BLOCKED             1             1
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16*          DONE   RUN:io_done             1          
 17           DONE       RUN:cpu             1          

Stats: Total Time 17
Stats: CPU Busy 9 (52.94%)
Stats: IO Busy  11 (64.71%)

### -S SWITCH_ON_END
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7        BLOCKED         READY                           1
  8*   RUN:io_done         READY             1          
  9        RUN:cpu         READY             1          
 10           DONE        RUN:io             1          
 11           DONE       BLOCKED                           1
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16*          DONE   RUN:io_done             1          
 17           DONE        RUN:io             1          
 18           DONE       BLOCKED                           1
 19           DONE       BLOCKED                           1
 20           DONE       BLOCKED                           1
 21           DONE       BLOCKED                           1
 22           DONE       BLOCKED                           1
 23*          DONE   RUN:io_done             1          
 24           DONE       RUN:cpu             1          

Stats: Total Time 24
Stats: CPU Busy 9 (37.50%)
Stats: IO Busy  15 (62.50%)

```
- Analysis / 分析:default中在time2，time9，time10，time16这几处因为统计错误导致整个预测表的CPU管道和I/O管道占用错误，并且在执行PID 0管道时，RUN：io_done后应该立刻执行PID 0的RUN:cpu命令，而不是先执行PID 1的RUN:io_done和RUN:io命令，此时的管道被PID 0占用，PID 1无法使用，但CPU处于准备状态，故此时PID 1应为READY状态。没有使用 -I 和 -S命令行。在使用 -I IO_RUN_IMMEDIATE命令行时，由于进行完RUN:io后直接运行另一个PID的RUN:cpu指令，故输出结果和我的预测结果一致（CPU Busy、IO Busy不一致，因为我的这两列是错误的）。在使用 -S SWITCH_ON_END命令行时由于需要在PID 0和PID 1各自的的管道内的I/O阻塞全部运行完成才可以运行各自的RUN：cpu命令。，所以Total time = 24。CPU Busy=9,IO Busy=15.CPU占用率为9/24≈37.50%，IO占用率为15/24≈62.50%。