### Before running:
	echo core >/proc/sys/kernel/core_pattern
	AFL_SKIP_CPUFREQ=1 

### Configure w/ AFL: 
	./configure CC=afl-cc CFLAGS="-O0 -g -fsanitize=address" LDFLAGS="-g -fsanitize=address -fsanitize=undefined" --prefix=/home/shubham/workspace/slurm2108/insdir  --sysconfdir=/home/shubham/workspace/slurm2108/conf 

### Running AFL:
	AFL_SKIP_CPUFREQ=1 afl-fuzz -i /home/shubham/worksp/slurm-install/afl/tests/ -o /home/shubham/worksp/slurm-install/afl/output -m none -- /home/shubham/worksp/slurm-install/installdir/bin/squeue -u @@
	AFL_SKIP_CPUFREQ=1 afl-fuzz -i /home/deadcow/Workspace/slurm2108/afl/tests/ -o /home/deadcow/Workspace/slurm2108/afl/output -m none -- /home/deadcow/Workspace/slurm2108/insdir/bin/sbatch @@
	
### ASAN Options:
	export ASAN_OPTIONS="detect_odr_violation=0:detect_leaks=0:abort_on_error=1:allow_user_segv_handler=0:handle_abort=1:symbolize=0"

### AFL-Cov: 
	export AFL_USE_ASAN=1    
    export ASAN_OPTIONS="detect_odr_violation=0:detect_leaks=0:abort_on_error=1:allow_user_segv_handler=0:handle_abort=1:symbolize=0"
    ./configure CC=afl-gcc CFLAGS="-O0 -g -fprofile-arcs -ftest-coverage -fsanitize=address,undefined" LDFLAGS="-g -fsanitize=address -fsanitize=undefined" --prefix=/home/shubham/workspace/slurm2108/insdir  --sysconfdir=/home/shubham/workspace/slurm2108/conf     
    ./afl-cov/afl-cov -d ./afl/output --live --coverage-cmd "LD_LIBRARY_PATH=/home/shubham/workspace/slurm2108/insdir/lib /home/shubham/workspace/slurm2108/insdir/bin/sbatch -f AFL_FILE" --code-dir .