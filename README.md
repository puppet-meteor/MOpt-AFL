# MOpt-AFL
### 1. Description
MOpt-AFL is a AFL-based fuzzer that utilizes a customized Particle Swarm Optimization (PSO) algorithm to find the optimal selection probability distribution of operators with respect to fuzzing effectiveness. More details can be found in the technical report. The installation of MOpt-AFL is the same as AFL's. 

### 2. Cite Information
Chenyang Lyu, Shouling Ji, Chao Zhang, Yuwei Li, Wei-Han Lee, Yu Song and Raheem Beyah, MOPT: Optimized Mutation Scheduling for Fuzzers, USENIX Security 2019. 

### 3. Seed Sets
We open source all the seed sets used in the paper "MOPT: Optimized Mutation Scheduling for Fuzzers".

### 4. Experiment Results
The experiment results can be found in https://drive.google.com/drive/folders/184GOzkZGls1H2NuLuUfSp9gfqp1E2-lL?usp=sharing.  We only open source the crash files since the space is limited. 

### 5. Technical Report
MOpt_TechReport.pdf is the technical report of the paper "MOPT: Optimized Mutation Scheduling for Fuzzers", which contains more deatails. 

### 6. Parameter Introduction

<del>Most important, you must add the parameter `-L` (e.g., `-L 0`) to launch the MOpt scheme. </del>

<br>`-L` controls the time to move on to the pacemaker fuzzing mode.
<br>`-L t:` when MOpt-AFL finishes the mutation of one input, if it has not discovered any new unique crash or path for more than t min, MOpt-AFL will enter the pacemaker fuzzing mode. 

<br>Setting 0 will enter the pacemaker fuzzing mode at first, which is recommended in a short time-scale evaluation (like 2 hours). 
<del><br>For instance, it may take three or four days for MOpt-AFL to enter the pacemaker fuzzing mode when `-L 30`. </del>

Hey guys, I realize that most experiments may last no longer than 24 hours. You may have trouble selecting a suitable value of 'L' without testing. So I modify the code in order to employ '-L 1' as the default setting. This means you do not have to add the parameter 'L' to launch the MOpt scheme. If you wish, provide a parameter '-L t' in the cmd can adjust the time when MOpt will enter the pacemaker fuzzing mode as aforementioned. Whether MOpt enters the pacemaker fuzzing mode has a great influence on the fuzzing performance in some cases as shown in our paper. 
<br>'-L 1' may not be the best choice  but will be acceptable in most cases. I may provide several experiment results to show this situation. 


 
 ###### The unique paths found by different fuzzing settings in 24 hours. 

 |Fuzzing setting |   infotocap @@ -o /dev/null | objdump -S @@  | sqlite3
 |:----: | :-----: | :------: |  :------:
 |MOpt -L 0  |  3629 | 5106 | 10498 
 |MOpt -L 1  | 3983 | 5499 | 9975 
 |MOpt -L 5  | 3772 | 2512 | 9332 
 |MOpt -L 10  | 4062 | 4741 | 9465 
 |MOpt -L 30  | 3162 | 1991 | 6337 
 |AFL  | 1821 | 1099 | 4949 



Other important parameters can be found in afl-fuzz.c, for instance, 
<br>`swarm_num:` the number of the PSO swarms used in the fuzzing process.
<br>`period_pilot:` how many times MOpt-AFL will execute the target program in the pilot fuzzing module, then it will enter the core fuzzing module. 
<br>`period_core:` how many times MOpt-AFL will execute the target program in the core fuzzing module, then it will enter the PSO updating module. 
<br>`limit_time_bound:` control how many interesting test cases need to be found before MOpt-AFL quits the pacemaker fuzzing mode and reuses the deterministic stage. 
0 < `limit_time_bound` < 1, MOpt-AFL-tmp.  `limit_time_bound` >= 1, MOpt-AFL-ever. 

Having fun with MOpt-AFL. 

### Citation:
```
@inproceedings {236282,
author = {Chenyang Lyu and Shouling Ji and Chao Zhang and Yuwei Li and Wei-Han Lee and Yu Song and Raheem Beyah},
title = {{MOPT}: Optimized Mutation Scheduling for Fuzzers},
booktitle = {28th {USENIX} Security Symposium ({USENIX} Security 19)},
year = {2019},
isbn = {978-1-939133-06-9},
address = {Santa Clara, CA},
pages = {1949--1966},
url = {https://www.usenix.org/conference/usenixsecurity19/presentation/lyu},
publisher = {{USENIX} Association},
month = aug,
}
```
