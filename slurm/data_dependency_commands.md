### LLVM_Building:

## Making LLVM from Source:
	git clone
	git checkout 14.0.6
	cmake -G "Unix Makefiles" -DLLVM_ENABLE_PROJECTS="clang;lld" -DLLVM_ENABLE_RUNTIMES="libcxx;libcxxabi" -DLLVM_BINUTILS_INCDIR=/home/deadcow/Workspace/binutils/include -DCMAKE_BUILD_TYPE=Release ../llvm/ 

#### Uninstalling LLVM:
	sudo xargs rm < install_manifest.txt

## Building Slurm with LLVM to create IR: 
##### Using ld.gold:
	./configure CC=clang CXX=clang++ RANLIB=llvm-ranlib CFLAGS=" -flto -std=gnu99 " LDFLAGS=" -flto -fuse-ld=gold " --prefix=/home/deadcow/Workspace/slurm2108test/insdir  --sysconfdir=/home/deadcow/Workspace/slurm2108test/conf

##### Without ld.gold:
	./configure CC=clang CXX=clang++ RANLIB=llvm-ranlib CFLAGS=" -flto -std=gnu99 " LDFLAGS=" -flto " --prefix=/home/deadcow/Workspace/slurm2108test/insdir  --sysconfdir=/home/deadcow/Workspace/slurm2108test/conf

##### Replacing LDFLAGS in Makefiles (if needed):
	LDFLAGS =  -flto -fuse-ld=gold -Wl,-plugin-opt=emit-llvm
	LDFLAGS =  -flto -fuse-ld=gold 

##### Replacing Linker (gnu ld or ld.gold) with lld (ld.lld)
	sudo mv /usr/bin/ld /usr/bin/ld.backup
	sudo rm /usr/bin/ld
	sudo ln -s /usr/bin/ld/lld /usr/bin/ld

#### Building Binutils with ld.gold:
	./configure CC=clang CXX=clang++ RANLIB=llvm-ranlib CFLAGS=" -flto -std=gnu99 " LDFLAGS=" -flto -fuse-ld=gold" --prefix=/home/deadcow/Workspace/binutils2/insdir

##### Building Slurm with ld.lld and dump IR:
	./configure CC=clang CXX=clang++ AR=llvm-ar NM=llvm-nm RANLIB=llvm-ranlib CFLAGS=" -flto -std=gnu99 " LDFLAGS=" -flto -fuse-ld=lld -save-temps -Wl,-plugin-opt=save-temps " --prefix=/home/deadcow/Workspace/slurm2108test/insdir  --sysconfdir=/home/deadcow/Workspace/slurm2108test/conf

	./configure CC=clang CXX=clang++ AR=llvm-ar NM=llvm-nm RANLIB=llvm-ranlib CFLAGS=" -flto -std=gnu99 " LDFLAGS=" -flto -fuse-ld=lld -save-temps -Wl,-plugin-opt=save-temps " --prefix=/home/deadcow/Workspace/slurm21/insdir  --sysconfdir=/home/deadcow/Workspace/slurm21/conf

##### Need to remove these two from Makefile when building for IR with lld and save-temps:
	src/scrontab
	src/plugins/openapi

#### Building Slurm in PCA VM using LLVM-7 and lld-8:
./configure CC=clang CXX=clang++ AR=llvm-ar NM=llvm-nm RANLIB=llvm-ranlib CFLAGS=" -flto " LDFLAGS=" -flto -Wl,-plugin-opt=save-temps "

### Running_passes:
	clang -emit-llvm -S -I/home/shubham/workspace/dependencyanalysis/slurm15 job_submit.c
	opt job_submit.ll -passes=dot-ddg -debug-pass-manager
	clang -O0 -Xclang -disable-O0-optnone -I/home/shubham/workspace/dependencyanalysis/slurm15 job_submit.c -emit-llvm -S -o job_submit.ll	
	clang-13 -O0 -Xclang -I/home/shubham/workspace/dependencyanalysis/slurm15 job_scheduler.c -emit-llvm -S -o job_scheduler.ll
	opt job_submit.ll -disable-output  -passes=dot-ddg	
	dot -Tps ddg.job_submit_plugin_init.dot -o outfile.ps	
	opt -print-memdeps -analyze -enable-new-pm=0 slurmctld_plugstack.ll
	test.c:
		clang -fno-discard-value-names -O0 -Xclang -disable-O0-optnone test.c -emit-llvm -S -o test.ll
		opt -print-memdeps -analyze -enable-new-pm=0 test.ll	
	opt -load /home/shubham/workspace/dependencyanalysis/test/llvm-pass-skeleton-/build/MemDepPrinter/libMemDepPrinterPass.so -pdep -S  -enable-new-pm=0 -analyze test.ll
	MemDepPrinter:		
		clang -fno-discard-value-names -O0 -Xclang -disable-O0-optnone test.c -emit-llvm -S -o test.ll
		opt -print-memdeps -analyze -enable-new-pm=0 test.ll
		opt -enable-tbaa -tbaa -basicaa -libcall-aa -scev-aa -globalsmodref-aa -domtree -memdep -print-memdeps -gvn -analyze -enable-new-pm=0 test.ll
		opt -enable-tbaa -tbaa  -print-memdeps -gvn -analyze -enable-new-pm=0 test.ll	
	llvm_DFGPass:
		clang -fno-discard-value-names -O0 -Xclang -disable-O0-optnone test.c -emit-llvm -S -o test.ll		
		opt -load /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/build/DFG/libDFGPass.so -DFGPass -enable-new-pm=0 /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/test.ll		
		opt -load /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/build/DFG/libDFGPass.so -DFGPass -enable-new-pm=0 /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/test.ll -o /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/main.dot		
		opt -instnamer -load /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/build/DFG/libDFGPass.so -DFGPass -enable-new-pm=0 /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/test.ll -o /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/main.dot		
		dot -Tps /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/main.dot -o /home/shubham/workspace/dependencyanalysis/llvm_DFGPass/outfile.png
	
	
