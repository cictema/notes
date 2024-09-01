Scheduling:
	Default: FIFO
	If Multifactor Priority Plugin used: ordered by priority
		If Backfill used: start low priority jobs if increasing efficiency
		
	
	Multifactor: Uses an equationf for Job_priority - uses factors and weights
	
		     Factors b/w 0 and 1
		     
		     Weights defined in sconf
		     
		     AgeFactor: longer in queue, more likely to start (not counting dependencies)
		     
		     QOS: TBD
		     
		     Partition: defined in sconf
		     
		     Size: priority proportional to size
		     
		     Fairshare: priotize under-serviced accounts, vice versa
		     		-> associations: combination of cluster + account + user names
		     		-> FairTree
		     		-> slurm account tree?
		     		-> algo:
		     			user_assoc_count?
		     		-> user share calculated using account tree
		     		->resource calculated using decay (formula)