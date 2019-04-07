# MOpt-AFL
Seed sets used in the paper "MOpt: Optimize Mutation Scheduling for Fuzzers"

The experiment results can be found in https://drive.google.com/drive/folders/184GOzkZGls1H2NuLuUfSp9gfqp1E2-lL?usp=sharing.  We only open source the crash files since the space is limited. 

MOpt_TechReport: the technical report of the paper "MOpt: Optimize Mutation Scheduling for Fuzzers"



MOpt-AFL is a AFL-based fuzzer that utilizes a customized Particle Swarm Optimization (PSO) algorithm to find the optimal selection probability distribution of operators with respect to fuzzing effectiveness. More details can be found in the technical report. 
The installation of MOpt-AFL is the same as AFL's. 

Note that you must add the parameter "-L" (e.g., "-L 0") to launch the MOpt scheme. 
"-L" controls the time to move on to the pacemaker fuzzing mode.
"-L t": when MOpt-AFL finishes the mutation of one input, if it has not discovered any new unique crash or path for more than t min, MOpt-AFL will enter the pacemaker fuzzing mode. 
Setting 0 will enter the pacemaker fuzzing mode at first, which is recommended in a short time-scale evaluation. 

Most important parameters can be found in afl-fuzz.c, for instance 
swarm_num: the number of the PSO swarms used in the fuzzing process.
period_pilot: how many times MOpt-AFL will execute the target program in the pilot fuzzing module, then it will enter the core fuzzing module. 
period_core: how many times MOpt-AFL will execute the target program in the core fuzzing module, then it will enter the PSO updating module. 

Having fun with MOpt-AFL. 