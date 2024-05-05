## PRE
* Download it from  https://downloadmirror.intel.com/793598/l_onemklbench_p_2024.0.0_49515.tgz
```
wget  https://downloadmirror.intel.com/793598/l_onemklbench_p_2024.0.0_49515.tgz
```
* uncompress it.hpl is on path/to/benchmarks_2024.0/linux/share/mkl/benchmarks/mp_linpack
* The machine have oneapi@2024.0. It can download on https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?operatingsystem=linux&distributions=online.
* set oneapi variables.
```
source /path/to/oneapi/setvars.sh
```
* Edit the runme_intel_prv_dynamic,we set MPI_PROC_NUM=4, MPI_PER_NODE=2.
## TUNNING
* we have one file host24, we wrote nodes in it.
* We have two scripts.
```
adj_par.sh
adj_power.sh
```
* $N = sqrt((memory(Bytes) * nodes)/8) * p(p=[0.8,0.9])$, due to this formula.we use adj_par.sh find best N.
 * due to readme.txt in intel ,we use adj_par.sh to find best NB.
 * we change PQ to find the best.
 * we try to set BCAST=1 to find better one.