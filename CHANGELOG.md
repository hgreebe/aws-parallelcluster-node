aws-parallelcluster-node CHANGELOG
===================================

This file is used to list changes made in each version of the aws-parallelcluster-node package.

3.11.0
------

**BUG FIXES**
- Fix a bug that was causing `clustermgtd` to fail when a field returned by the command `scontrol show nodes`
  has a value that contains the equal (`=`) character.

3.10.1
------

**CHANGES**
- There were no changes for this version.

3.10.0
------

**CHANGES**
- There were no changes for this version.

3.9.3
------

**CHANGES**
- There were no changes for this version.

3.9.2
------

**CHANGES**
- There were no changes for this version.

3.9.1
------

**CHANGES**
- There were no changes for this version.

3.9.0
------

**ENHANCEMENTS**
- Add a clustermgtd config option `ec2_instance_missing_max_count` to allow a configurable amount of retries for eventual EC2 
  describe instances consistency with run instances

**CHANGES**

**BUG FIXES**

3.8.0
------

**ENHANCEMENTS**
- Add support for EC2 Capacity Blocks for ML.

**CHANGES**
- Perform job-level scaling by default for all jobs, using information in the `SLURM_RESUME_FILE`. Job-level scaling
  can be disabled using new `job_level_scaling` resume configuration parameter.
- Remove support of `all_or_nothing_batch` configuration parameter in the Slurm resume program, in favor of the new `Scheduling/ScalingStrategy` cluster configuration.

3.7.2
------

**CHANGES**
- There were no changes for this version.

3.7.1
------

**CHANGES**
- There were no changes for this version.

3.7.0
------

**CHANGES**
- Perform default job-level scaling for exclusive jobs, by reading job information from `SLURM_RESUME_FILE`.
- Make `aws-parallelcluster-node` daemons handle only ParallelCluster-managed Slurm partitions.

**BUG FIXES**
- Fix an issue that was causing misalignment of compute nodes DNS name on instances with multiple network interfaces,
  when using `SlurmSettings/Dns/UseEc2Hostnames` equals to `True`.

3.6.1
------

**CHANGES**
- Avoid duplication of nodes seen by ClusterManager if compute nodes are added to multiple Slurm partitions.

**BUG FIXES**
- Fix fast insufficient capacity fail-over logic when using Multiple Instance Types and no instances are returned

3.6.0
------

**CHANGES**
- Consider dynamic nodes failing Slurm registration, identified by `INVALID_REG` flag, as bootstrap failure towards the Slurm protected mode.
  Static nodes failing the Slurm registration are already treated as a bootstrap failure after the `node_replacement_timeout`.

**BUG FIXES**
- Fix an issue that was causing misalignment of compute nodes IP on instances with multiple network interfaces.

3.5.1
------

**BUG FIXES**
- Fix for compute_console_output log file being truncated at every clustermgtd iteration.

3.5.0
------

**ENHANCEMENTS**
- Add logging of compute node console output to CloudWatch from head node on compute node bootstrap failure.
- Add validators to prevent malicious string injection while calling the subprocess module.

**BUG FIXES**
- Fix an issue in clustermgtd that caused compute nodes rebooted via Slurm to be replaced if the EC2 instance status checks fail.

3.4.1
------

**CHANGES**
- There were no changes for this version.

3.4.0
------

**ENHANCEMENTS**
- Add support for launching nodes across multiple availability zones to increase capacity availability.


**CHANGES**
- Do not consider dynamic nodes in IDLE+CLOUD+COMPLETING+POWER_DOWN+NOT_RESPONDING as unhealthy anymore.
  - The root cause has been fixed in Slurm 22.05.6.

3.3.0
------

**ENHANCEMENTS**
- Add support for EC2 Fleet as an alternative instance provisioning mechanism that allows greater flexibility in terms of instance diversification and launch strategy.

**CHANGES**
- Do not replace DRAIN nodes when nodes are in COMPLETING state as Epilog may be still running.
- Consider all dynamic nodes in IDLE+CLOUD+COMPLETING+POWER_DOWN+NOT_RESPONDING as unhealthy.

3.2.1
------

**CHANGES**
- There were no changes for this version.

3.2.0
------

**ENHANCEMENTS**
- Temporarily disable compute resource when a node launch fails due to insufficient capacity.
- Add support for rebooting compute nodes via Slurm.

**CHANGES**
- Drop support for python 3.6.
- Do not replace dynamic node in POWER_DOWN as jobs may be still running.
- Manage static nodes in POWERING_DOWN.
- Automatic disabling of the compute fleet when the configuration parameter `Scheduling/SlurmQueues/ComputeResources/SpotPrice`
  is lower than the minimum required Spot request fulfillment price.

**BUG FIXES**
- Handle corner case in the scaling logic when instance is just launched and the describe instances API doesn't report yet all the EC2 info.
- Fix file handle leak in `computemgtd`.

3.1.5
------

**CHANGES**
- There were no changes for this version.

3.1.4
------

**BUG FIXES**
- Reset node address when setting slurm unhealthy static node to down to avoid treating static node failed with insufficient capacity as bootstrap failure node.

3.1.3
------

**CHANGES**
- There were no changes for this version.

3.1.2
------

**CHANGES**
- There were no changes for this version.

3.1.1
------

**ENHANCEMENTS**
- Add possibility to override EC2 RunInstances parameters for instances launched in a Slurm cluster.

**CHANGES**
- Update Slurm plugin to support version 21.08.

3.0.3
------

**CHANGES**
- There were no changes for this version.

3.0.2
------

**CHANGES**
- There were no changes for this version.

3.0.1
------

**CHANGES**
- There were no changes for this version.

3.0.0
------

**ENHANCEMENTS**
- Implement scaling protection mechanism with Slurm scheduler: compute fleet is automatically set to 'PROTECTED' state
  in case recurrent failures are encountered when provisioning nodes.
- Implement `computemgtd` self-termination via `shutdown` command instead of calling TerminateInstances.

**CHANGES**
- Drop support for SGE and Torque schedulers.
- Use tags prefix `parallelcluster:` when describing EC2 instances.
- Run Slurm command `scontrol` with sudo because clustermgtd is executed as cluster admin user (not root).

2.11.3
-----

**CHANGES**
- There were no notable changes for this version.

2.11.2
-----

**BUG FIXES**
- Slurm: fix issue that prevented powering-up nodes to be correctly reset after a stop and start of the cluster.

2.11.1
-----

**CHANGES**
- There were no notable changes for this version.

2.11.0
-----

**ENHANCEMENTS**
- SGE: always use shortname as hostname filter with `qstat`. This will make nodewatcher more robust when using custom DHCP option, where the full hostname seen by `SGE` might differ from the hostname returned from EC2 metadata(local-hostname).
- Transition from IMDSv1 to IMDSv2.
- Have `computemgtd` reuse last available daemon configuration when the new one cannot be loaded.
- Use methods with timeouts to read NFS shared files, which will prevent `computemgtd` from hanging when NFS filesystems are not available.

**BUG FIXES**
- Fix a bug that caused `clustermgtd` to not immediately replace instances with failed status check that are in replacement process.

2.10.4
-----

**CHANGES**
- There were no notable changes for this version.

2.10.3
-----

**CHANGES**
- There were no notable changes for this version.

2.10.2
-----

**CHANGES**
- There were no notable changes for this version.

2.10.1
-----

**ENHANCEMENTS**
- Improve error handling in slurm plugin processes when clustermgtd is down.

**CHANGES**
- Increase max attempts when retrying on Route53 API call failures.

2.10.0
-----
**ENHANCEMENTS**
- Add new `all_or_nothing_batch` configuration parameter for `slurm_resume` script. When `True`, `slurm_resume` will
  succeed only if all the instances required by all the pending jobs in Slurm will be available.

**CHANGES**
- CentOS 6 is no longer supported.
- Optimize retrieval of nodes info from Slurm scheduler.
- Improve retrieval of instance type info by using `DescribeInstanceType` API.
- Increase timeout from 10 to 30 seconds when `clustermgtd` and `computemgtd` daemons invoke Slurm commands.

**BUG FIXES**
- Retrieve the right number of compute instance slots when instance type is updated.
- Fix a bug that was causing `clustermgtd` and `computemgtd` sleep interval to be incorrectly computed when
  system timezone is not set to UTC.

2.9.1
-----

**CHANGES**
- There were no notable changes for this version.

2.9.0
-----
**ENHANCEMENTS**
- Add support for multiple queues and multiple instance types feature with the Slurm scheduler.
  - Replace the previously available scaling components with: `clustermgtd` daemon that
    takes care of handling compute fleet management operations, included the processing of health check failures coming
    from EC2 and cluster start/stop operations; `slurm_resume` and `slurm_suspend` scripts that integrate with Slurm
    power saving plugin; `computemgtd` daemon that monitors the health of the system from the compute nodes.
  - Replace Auto Scaling Group with plain EC2 APIs when provisioning cluster nodes.
  - Register cluster nodes in a Route53 private hosted zone when DNS resolution is enabled for the cluster.
  - Register mapping between Slurm node names and EC2 instances in DynamoDB table.
  - Create log files for the new components in `/var/log/parallelcluster/` dir.

2.8.1
-----

**CHANGES**
- There were no notable changes for this version.

2.8.0
-----

**ENHANCEMENTS**
- Dynamically generate the architecture-specific portion of the path to the SGE binaries directory.

2.7.0
-----

**ENHANCEMENTS**
- `sqswatcher`: The daemon is now compatible with VPC Endpoints so that SQS messages can be passed without traversing
  the public internet.

2.6.1
-----

**ENHANCEMENTS**
- Improved the management of SQS messages and retries to speed-up recovery times when failures occur.

**CHANGES**
- Do not launch a replacement for an unhealthy or unresponsive node until this is terminated. This makes cluster slower
  at provisioning new nodes when failures occur but prevents any temporary over-scaling with respect to the expected
  capacity.
- Increase parallelism when starting `slurmd` on compute nodes that join the cluster from 10 to 30.
- Reduce the verbosity of messages logged by the node daemons.
- Do not dump logs to `/home/logs` when nodewatcher encounters a failure and terminates the node. CloudWatch can be
  used to debug such failures.
- Reduce the number of retries for failed REMOVE events in sqswatcher.

**BUG FIXES**
- Fixed a bug in the ordering and retrying of SQS messages that was causing, under certain circumstances of heavy load,
  the scheduler configuration to be left in an inconsistent state.
- Delete from queue the REMOVE events that are discarded due to hostname collision with another event fetched as part
  of the same `sqswatcher` iteration.


2.6.0
-----

**CHANGES**
- Remove logic that was adding compute nodes identity to known_hosts file for all OSs except CentOS6

**BUG FIXES**
- Fix Torque issue that was limiting the max number of running jobs to the max size of the cluster.


2.5.1
-----

**BUG FIXES**
- Fix bug in sqswatcher that was causing the daemon to crash when more than 100 DynamoDB tables are present in the
  cluster region.


2.5.0
-----

**ENHANCEMENTS**
- Slurm:
  - Add support for scheduling with GPU options. Currently supports the following GPU-related options: `—G/——gpus,
    ——gpus-per-task, ——gpus-per-node, ——gres=gpu, ——cpus-per-gpu`.
  - Add gres.conf and slurm_parallelcluster_gres.conf in order to enable GPU options. slurm_parallelcluster_gres.conf
    is automatically generated by node daemon and contains GPU information from compute instances. If need to specify
    additional GRES options manually, please modify gres.conf and avoid changing slurm_parallelcluster_gres.conf when
    possible.
  - Integrated GPU requirements into scaling logic, cluster will scale automatically to satisfy GPU/CPU requirements
    for pending jobs. When submitting GPU jobs, CPU/node/task information is not required but preferred in order to
    avoid ambiguity. If only GPU requirements are specified, cluster will scale up to the minimum number of nodes
    required to satisfy all GPU requirements.
  - Slurm daemons will now keep running when cluster is stopped for better stability. However, it is not recommended
    to submit jobs when the cluster is stopped.
  - Change jobwatcher logic to consider both GPU and CPU when making scaling decision for slurm jobs. In general,
    cluster will scale up to the minimum number of nodes needed to satisfy all GPU/CPU requirements.
- Reduce number of calls to ASG in nodewatcher to avoid throttling, especially at cluster scale-down.

**CHANGES**
- Increase max number of SQS messages that can be processed by sqswatcher in a single batch from 50 to 200. This
  improves the scaling time especially with increased ASG launch rates.
- Increase faulty node termination timeout from 1 minute to 5 in order to give some additional time to the scheduler
  to recover when under heavy load.

**BUG FIXES**
- Fix jobwatcher behaviour that was marking nodes locked by the nodewatcher as busy even if they had been removed
  already from the ASG Desired count. This was causing, in rare circumstances, a cluster overscaling.
- Better handling of errors occurred when adding/removing nodes from the scheduler config.
- Fix bug that was causing failures in sqswatcher when ADD and REMOVE event for the same host are fetched together.


2.4.1
-----

**ENHANCEMENTS**
- Torque:
  - process nodes added to or removed from the cluster in batches in order to speed up cluster scaling.
  - scale up only if required slots/nodes can be satisfied
  - scale down if pending jobs have unsatisfiable CPU/nodes requirements
  - add support for jobs in hold/suspended state (this includes job dependencies)
  - automatically terminate and replace faulty or unresponsive compute nodes
  - add retries in case of failures when adding or removing nodes
  - add support for ncpus reservation and multi nodes resource allocation (e.g. -l nodes=2:ppn=3+3:ppn=6)

**CHANGES**
- Drop support for Python 2. Node daemons now support Python >= 3.5.
- Torque: trigger a scheduling cycle every 1 minute when there are pending jobs in the queue. This is done in order
  to speed up jobs scheduling with a dynamic cluster size.

**BUG FIXES**
- Restore logic that was automatically adding compute nodes identity to known_hosts file.
- Slurm: fix issue that was causing the daemons to fail when the cluster is stopped and an empty compute nodes file
  is imported in Slurm config.
- Torque: fix command to disable hosts in the scheduler before termination.


2.4.0
-----

**ENHANCEMENTS**
- Dynamically fetch compute instance type and cluster size in order to support updates
- SGE:
  - process nodes added to or removed from the cluster in batches in order to speed up cluster scaling.
  - scale up only if required slots/nodes can be satisfied
  - scale down if pending jobs have unsatisfiable CPU/nodes requirements
  - add support for jobs in hold/suspended state (this includes job dependencies)
  - automatically terminate and replace faulty or unresponsive compute nodes
  - add retries in case of failures when adding or removing nodes
- Slurm:
  - scale up only if required slots/nodes can be satisfied
  - scale down if pending jobs have unsatisfiable CPU/nodes requirements
  - automatically terminate and replace faulty or unresponsive compute nodes
- Dump logs of replaced failing compute nodes to shared home directory

**CHANGES**
- SQS messages that fail to be processed are re-queued only 3 times and not forever
- Reset idletime to 0 when the host becomes essential for the cluster (because of min size of ASG or because there are
  pending jobs in the scheduler queue)
- SGE: a node is considered as busy when in one of the following states "u", "C", "s", "d", "D", "E", "P", "o".
  This allows a quick replacement of the node without waiting for the `nodewatcher` to terminate it.

**BUG FIXES**
- Slurm: add "BeginTime", "NodeDown", "Priority" and "ReqNodeNotAvail" to the pending reasons that trigger
  a cluster scaling
- Add a timeout on remote commands execution so that the daemons are not stuck if the compute node is unresponsive
- Fix an edge case that was causing the `nodewatcher` to hang forever in case the node had become essential to the
  cluster during a call to `self_terminate`.


2.3.1
-----

**BUG FIXES**
- `sqswatcher`: Slurm - Fix host removal


2.3.0
-----

**CHANGES**
- `sqswatcher`: Slurm - dynamically adjust max cluster size based on ASG settings
- `sqswatcher`: Slurm - use FUTURE state for dummy nodes to prevent Slurm daemon from contacting unexisting nodes
- `sqswatcher`: Slurm - dynamically change the number of configured FUTURE nodes based on the actual nodes that
  join the cluster. The max size of the cluster seen by the scheduler always matches the max capacity of the ASG.
- `sqswatcher`: Slurm - process nodes added to or removed from the cluster in batches. This speeds up cluster scaling
  which is able to react with a delay of less than 1 minute to variations in the ASG capacity.
- `sqswatcher`: Slurm - add support for job dependencies and pending reasons. The cluster won't scale up if the job
  cannot start due to an unsatisfied dependency.
- Slurm - set `ReturnToService=1` in scheduler config in order to recover instances that were initially marked as down
  due to a transient issue.
- `sqswatcher`: remove DynamoDB table creation
- improve and standardize shell command execution
- add retries on failures and exceptions

**BUG FIXES**
- `sqswatcher`: Slurm - set compute nodes to DRAIN state before removing them from cluster. This prevents the scheduler
  from submitting a job to a node that is being terminated.

2.2.1
-----

**CHANGES**
- `nodewatcher`: sge - improve logic to detect if a compute node has running jobs
- `sqswatcher`: remove invalid messages from SQS queue in order to process remaining messages
- `sqswatcher`: add number of slots to the log of torque scheduler
- `sqswatcher`: add retries in case aws request limits are reached

**BUG FIXES**
- `sqswatcher`: keep processing compute node termination until all scheduled jobs are terminated/cancelled.
  This allows to automatically remove dead nodes from the scheduler once all jobs are terminated.
- `jobwatcher`: better handling of error conditions and usage of fallback values
- `nodewatcher`: enable daemon when cluster status is `UPDATE_ROLLBACK_COMPLETE`

**TOOLING**
- Add a script to simplify node package upload when using `custom_node_package` option

2.1.1
-----

- China Regions, cn-north-1 and cn-northwest-1 support

2.1.0
-----

Bug Fixes:
- Don't schedule jobs on compute nodes that are terminating

2.0.2
-----

- Align version to main ParallelCluster package

2.0.0
-----

- Rename package to AWS ParallelCluster


1.6.0
-----

Bug fixes/minor improvements:

- Changed scaling functionality to scale up and scale down faster.


1.5.4
-----

Bug fixes/minor improvements:

- Upgraded Boto2 to Boto3 package.


1.5.2
-----

Bug fixes/minor improvements:

- Fixed Slurm behavior to add CPU slots so multiple jobs can be scheduled on a single node, this also sets CPU as a consumable resource

1.5.1
-----

Bug fixes/minor improvements:

- Fixed Torque behavior when scaling up from an empty cluster
- Avoid Torque server restart when adding and removing compute nodes
