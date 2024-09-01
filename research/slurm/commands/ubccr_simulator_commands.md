mariadb:
If you are using ubuntu, I suggest you to use the apt-get command to remove the database package, for instance:

sudo apt-get purge mariadb*
sudo apt-get purge libmariadb*

You may check which mariadb packages are installed with:
sudo dpkg -l | grep mariadb 


./configure --prefix=/home/shubham/workspace/slurmsim/slurm_opt --enable-simulator \
--enable-pam --without-munge --enable-front-end --with-mysql-config=/etc/mysql/ --disable-debug \
CFLAGS="-g -O3 -D NDEBUG=1"



./configure --prefix=/home/shubham/workspace/slurmsim/slurm_opt --enable-simulator \
--enable-pam --without-munge --enable-front-end --with-mysql-config=/usr/bin/ --disable-debug \
CFLAGS="-g -O3 -D NDEBUG=1"



/home/shubham/workspace/slurmsim/slurm_sim_tools/src/cp_slurm_conf_dir.py -o -s /home/shubham/workspace/slurmsim/slurm_opt \
/home/shubham/workspace/slurmsim/slurm_sim_tools/tutorials/micro_cluster/etc /home/shubham/workspace/slurmsim/sim/micro/baseline

/home/shubham/workspace/slurmsim


sudo SLURM_CONF=/home/shubham/workspace/slurmsim/sim/micro/baseline/etc/slurm.conf /home/shubham/workspace/slurmsim/slurm_opt/sbin/slurmdbd -Dvvvv

DROP USER 'slurm'@'localhost';


mysql -u root -p
create user 'slurm'@'localhost' identified by 'slurm';
create database slurmdb_micro2sim

GRANT ALL PRIVILEGES ON slurmdb_micro2sim TO 'slurm'@'localhost' IDENTIFIED BY 'slurm';
GRANT ALL PRIVILEGES ON * . * TO 'slurm'@'localhost';








export SLURM_CONF=/home/shubham/workspace/slurmsim/sim/micro/baseline/etc/slurm.conf SACCTMGR=/home/shubham/workspace/slurmsim/slurm_opt/bin/sacctmgr




./configure --prefix=/home/slurm/slurm_sim_ws/slurm_opt --enable-simulator \
--enable-pam --without-munge --enable-front-end --with-mysql-config=/usr/bin/ --disable-debug \
CFLAGS="-g -O3 -D NDEBUG=1"



cd /home/shubham/workspace/slurmsim/sim/micro/baseline

/home/shubham/workspace/slurmsim/slurm_sim_tools/src/run_sim.py -e/home/shubham/workspace/slurmsim/sim/micro/baseline/etc -s /home/shubham/workspace/slurmsim/slurm_opt -d


/home/shubham/workspace/slurmsim/slurm_sim_tools/src/run_sim.py -e/home/shubham/workspace/slurmsim/sim/micro/baseline/etc -s /home/shubham/workspace/slurmsim/slurm_opt -t /home/shubham/workspace/slurmsim/jobtraces/features_test.trace



/home/shubham/workspace/slurmsim/slurm_sim_tools/src/run_sim.py \
-e /home/shubham/workspace/slurmsim/sim/micro/baseline/etc \
-s /home/shubham/workspace/slurmsim/slurm_opt \
-octld slurmctld.out -odbd slurmdbd.out \
-d -t /home/shubham/workspace/slurmsim/eventtrace/test.trace





ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '*';