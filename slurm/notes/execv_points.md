Execv(Improper Neutralization):
/home/shubham/workspace/slurm2108/src/slurmctld/controller.c:3494:  [4] (shell) execv:
  This causes a new program to execute and is difficult to use safely
  (CWE-78). try using a library call that implements the same functionality
  if available.
  
 
controller.c: 
3458:  static void _run_primary_prog(bool primary_on)
3494: 		execv(prog_name, argv);




945: static void  _init_config(void)
991: static void _reconfigure_slurm(void)
1007: rc = read_slurm_conf(2, true);
1006: lock_slurmctld(config_write_lock);


1035: static void *_slurmctld_signal_hand(void *no_data)
1068: reconfigure_slurm()


read_config.c:
545: static void _init_all_slurm_conf(void)
1069: int read_slurm_conf(int recover, bool reconfig)