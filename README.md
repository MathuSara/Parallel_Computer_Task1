# CSC770_task1

(1)	log in HPC computer using your account (Note that if you try to access hpc computer from offcampus, please connect to chizen.csi.cuny.edu, and then ssh to username@mhn).

(2)	Type the command of module load openmpi to load the mpi.

Similar output will be as the following:

[yumei.huo@login-0-1 ~]$ module load openmpi
Java module loaded - the system's JAVA is replaced by JDK 1.8.0_211

(3)	Type the command of module list to check the packages were loaded successfully.
Similar output will be as the following:

[yumei.huo@login-0-1 ~]$ module list

Currently Loaded Modules:
  1) cuda/10.1   2) java/1.8.211   3) binutils/2.28   4) gcc/7.3.0   5) openmpi/4.0.1_gnu


(4)	Create a new directory with the name of lab11 and then get into the directory. What will be the command?

(5)	Create a file with the name of hello.c. Use vi editor or other editors you are familiar with(emacs, vim…) to type a program which will print out “Hello World!”. Make sure that the program has two parameters argc and argv as we discussed in the class. What will be the program?

(6)	Now revise program hello.c such that MPI_Init and MPI_Finalize are included. (In order to use these two functions, what header file do you need? ) What will be the program now? 

(7)	Compile your program, using the following command:
 mpicc hello.c -o hello

(8)	create a job script named job1,  type command of vi job1, then type the following: 

#!/bin/bash
#SBATCH -J PartProdHello
#SBATCH --ntasks 40 
#SBATCH --nodes 5
#SBATCH --tasks-per-node 8
#SBATCH --mem-per-cpu 2GB
#SBATCH --partition partproduction 
#SBATCH --account=students

cd $SLURM_SUBMIT_DIR

mpirun -np 40 ./hello 



(note1: please test your script file, you may need to add the following line:
module load openmpi
and insert it right before the line of mpirun)

(note2: if your program cannot be executed, you can try to replace the last line (the line of mpirun) with the following;
mpirun --mca opal_warn_on_missing_libcuda 0 -np 40 ./hello

(9)	Then run command:
sbatch job1

Similar output will be as follows:

[yumei.huo@login-0-0 lab1]$ sbatch job1
Submitted batch job 125597

(10)	 run the following command to check the status of your program:
qstat or qstat -a

Similar output will be as follows:



(11)	When your program is finished, check in the current directory whether you have file named like slurm-xxxxx.out. Then open the file slurm-xxxxx.out and check the content. You should have the following content:

compute-0-96
Java module loaded - the system's JAVA is replaced by JDK 1.8.0_211
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>
Hello World<br>

"slurm-125594.out" 42L, 561C                                       
(12)	Now copy your program hello.c to a new file named hello_node.c. Revise program hello_node.c such that two variables nprocs and mypid are declared and will hold the value of total number of processes and identifier of calling process respectively. Then print out “Hello world from process xxx of total xxx!” What functions should you use to find the value for nprocs and mypid? What will be the program now? 

(13)	Compile your program, using the following command:
 	mpicc hello_node.c -o hello_node

(14)	Copy your job script job1 to a new script file named job2,  type command of vi job2, then change the script file so that you will request 8 tasks (4 nodes for example, then 2 cores of each node) and change the executable file to hello_node. 

(15)	Then run command:
sbatch job2

(16)	Run the following command to check the status of your program:
qstat

(17)	When your program is finished, check in the current directory whether you have the related output file named like slurm-xxxxx.out. Then open the file slurm-xxxxx.out and check the content. You should have the following content:

 Java module loaded - the system's JAVA is replaced by JDK 1.8.0_211

Hello World from process 0 of total 8!<br>
Hello World from process 2 of total 8!<br>
Hello World from process 1 of total 8!<br>
Hello World from process 3 of total 8!<br>
Hello World from process 4 of total 8!<br>
Hello World from process 5 of total 8!<br>
Hello World from process 6 of total 8!<br>
Hello World from process 7 of total 8!<br>


