### Retro Configuration

Retro reporting can be configured with the property `resource-reporting.aggregation.active`.  If set to false, Retro will not aggregate resource consumption.  Aggregation for individual resources can be configured with the `resource-reporting.aggregation.enabled.*` properties listed below.

The property `resource-reporting.reporting.interval` configures the reporting time interval in milliseconds.  Resource reports will be sent at this time interfal.

The following is the full listing of default config values

	// Retro Reporting Config
	resource-reporting {

	    # settings for the aggregation component of resource reporting.  consumption can be aggregated without reporting
	    aggregation {
	        active                          = true                  # aggregation is on by default
	        disk-cache-threshold            = 120000000             # bytes per second threshold for a disk read to be logged as a disk cache read
	        small-read                      = 131072                # a small read is one that is <= 128kb
	        seek-threshold                  = 10000000              # a small read is a seek if it is slower than 10 ms (10,000,000 ns)

	        # per-resource settings
	        enabled {
	            disk                        = ${resource-reporting.aggregation.active}  # disk aggregation is enabled by default; can set to true or false
	            disk-cache                  = ${resource-reporting.aggregation.active}  # disk-cache aggregation is enabled by default; can set to true or false
	            network                     = ${resource-reporting.aggregation.active}  # network aggregation is enabled by default; can set to true or false
	            cpu                         = ${resource-reporting.aggregation.active}  # cpu aggregation is enabled by default; can set to true or false
	            hdfs                        = ${resource-reporting.aggregation.active}  # hdfs aggregation is enabled by default; can set to true or false
	            locks                       = ${resource-reporting.aggregation.active}  # locks aggregation is enabled by default; can set to true or false
	            queue                       = ${resource-reporting.aggregation.active}  # queue aggregation is enabled by default; can set to true or false
	            throttlingpoint             = ${resource-reporting.aggregation.active}  # throttling point aggregation is enabled by default; can set to true or false
	            batch                       = ${resource-reporting.aggregation.active}  # batch aggregation is enabled by default; can set to true or false
	        }
	    }

	    # settings for the reporting side.  reporting can be disabled or modified separately from aggregation
	    reporting {
	        active                          = true                  # reporting is on by default. default reporter is zmq
	        interval                        = 1000                  # reporting interval in milliseconds

	        # reporting settings for the zmq reporter
	        zmq {
	            topics {
	                default                 = "default"             # default topic for reports if none configured
	                immediate               = "immediate"           # topic for immediate reports to be published on
	                disk                    = "disk"                # topic on which to report disk usage reports
	                disk-cache              = "diskcache"           # topic on which to report disk cache usage reports
	                network                 = "network"             # topic on which to report network usage reports
	                cpu                     = "cpu"                 # topic on which to report cpu usage reports
	                hdfs                    = "hdfs"                # topic on which to report hdfs usage reports
	                locks                   = "locks"               # topic on which to report locks usage reports
	                queue                   = "queue"               # topic on which to report queue usage reports
	                throttlingpoint         = "throttlingpoint"     # topic on which to report throttling point usage reports
	                batch                   = "batch"               # topic on which to report batch usage reports
	            } 
	        }

	        # reporting settings for the file printer reporter
	        printer {
	            filename                    = "hdfsreports.tsv"     # default filename for reports file
	        }
	    }

	}

	// Retro Resources Config
	resource-tracing {

	    disk {
	        sync-after-write        = false     # set to true to sync to disk after every file write
	        sync-threshold          = 0         # number of bytes written before disk sync. only valid if sync-on-write is true.  if 0, will sync after every write
	    }

	    background {
	        heartbeat               = -10       # tenant class for heartbeat background process; set to -1 to disable
	        replication             = -11       # tenant class for replication background process; set to -1 to disable
	        invalidate              = -12       # tenant class for deleting blocks from disk; set to -1 to disable
	        finalize                = -13       # tenant class for finalizing blocks; set to -1 to disable
	        recover                 = -14       # tenant class for recovering blocks; set to -1 to disable      
	    }

	    batch {
	        hbase {
	            fshlog              = -20       # tenant class for hbase fshlog; set to -1 to disable
	        }
	    }

	}

	// Retro Throtting Config
	retro {
	    throttling {
	        topic = "throttlingupdates"                 # topic on which the controller should publish throttling point rates
	        schedulertopic = "schedulerupdates"         # topic on which the controller should publish scheduler rates

	        default-throttlingpoint = "simple"          # type of throttling point to use.  valid choices: ["simple", "batched-<type>-<batchsize>", "default"]
	        throttlingpoints {
	            "point-example" = "batched-simple-5"    # configures the "point-example" throttling point to use the 'batched' throttling point type.
	        }

	        default-throttlingqueue = "locking"         # type of throttling queue to use.  valid choices: ["locking", "delay", "default"]
	        throttlingqueues {
	            "queue-example" = "delay"               # configures the "queue-example" throttling queue to use the "delay" throttling queue type.
	        }

	        default-scheduler = "mclock-3"              # default scheduler type to use. valid choices: ["mclock-<concurrency>", "default"]
	        schedulers {
	            "scheduler-example" = "mclock-5"        # configures the "schedulers-example" scheduler to use the "mclock" scheduler type with a concurrency of 5
	        }

	        debug {
	            mclock = false                          # set to true to print mclock debug messages
	        }
	    }
	}

	// Retro Visualization Server Config
	resource-reporting {
	    visualization {
	        webui-port = 4081
	    }
	}