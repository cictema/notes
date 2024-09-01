controller.c: 
292: 
	  _init_config();
	  -initilizes config data structure and fills with some info (does not read config)
293:
			_parse_commandline(argc, argv);
	        -has config options?
	        -can set config file name with -f flag
301 - 304:
	 if (!(conf_file = slurm_conf_filename))
	 if (!(conf_file = getenv("SLURM_CONF")))
	      conf_file = default_slurm_config_file;
    
	 -if config not set by command line argument, then sets conf_file env("SLURM_CONF"), if that is also not set, sets it to default_slurm_config_file (our actual config which is used) which is defined in global_defaults.c along with default_plugin_path which is created by the makefile
    -sets conf_file
                
304: 
	slurm_conf_init(conf_file);
          -is in read_config.c
          -inititate config;
              -establish config source through _establish_config_source(), 
                  - If config_file was defined (e.g., through the -f option to slurmd) or the SLURM_CONF variable, usees those
                     -otherwise default_slurm_config_file 
                     -otherwise tries try the SLURM_CONF_SERVER envvar or DNS SRV entries to fetch the configs from the slurmctld.
              - setenv("SLURM_CONF", config_file, 1);
                - sets environment variable to config filename of config
          -calls 3349: init_slurm_conf(conf_ptr);
              -fill with dummy values
          -calls 3350:_init_slurm_conf(config_file)
              - calls 3136: s_p_parse_file() to parse file and create hashtable
              - calls 3139: _validate_and_set_defaults() to validate and set configuration values
              - 4809: sets SlurmctldPrimaryOffProg SlurmctldPrimaryOnProg
                  	(void) s_p_get_string(&conf->slurmctld_primary_off_prog,
                              "SlurmctldPrimaryOffProg", hashtbl);
                    (void) s_p_get_string(&conf->slurmctld_primary_on_prog,
                              "SlurmctldPrimaryOnProg", hashtbl);
          -scontrol???