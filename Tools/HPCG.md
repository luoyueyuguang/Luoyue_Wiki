## The best result
* The best result is n192-4p-24t_V3.1_2024-01-09_15-05-07.txt
## PRE
* Download it from  https://downloadmirror.intel.com/793598/l_onemklbench_p_2024.0.0_49515.tgz
```
wget  https://downloadmirror.intel.com/793598/l_onemklbench_p_2024.0.0_49515.tgz
```
* uncompress it.hpcg is on path/to/benchmarks_2024.0/linux/share/mkl/benchmarks/hpcg
* `cd bin/`
* The machine have oneapi@2024.0. It can download on https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?operatingsystem=linux&distributions=online.
* set oneapi variables.
```
source /path/to/oneapi/setvars.sh
```
* set OMP_NUM_THREADS
* run it with follow commands
```
mpiexec.hydra -genvall --hostfile host24 -n $nprocs -ppn $nprocs_per_node  ./xhpcg_avx2
```
## TUNNING
* use run.sh to find best nx=ny=nz.
* set different OMP_NUM_THREADS.
* we try to use different memory allocator.
```
LD_PRELOAD=*malloc.so mpiexec.hydra -genvall --hostfile host24 -n $nprocs -ppn $nprocs_per_node  ./xhpcg_avx2
```