# bjobs(1)

**bjobs**


Displays and filters information about LSF jobs. Specify one or
more job IDs (and, optionally, an array index list) to display
information about specific jobs (and job arrays).



# Synopsis



**bjobs** [options] [job_ID |
**"**job\_ID**[**index\_list**]"** ... ]

**bjobs** **-h**[**elp**] [**all**] [**description**]
[category_name ...] [**-**option_name ...]

**bjobs** **-V**


# Categories and Options



Use the keyword all to display all options and the keyword
description to display a detailed description of the bjobs
command. For more details on specific categories and options,
specify bjobs -h with the name of the categories and options.

   Categories  


   Options  
   List of options for the bjobs command.

   Description  
   By default, displays information about your own pending,
   running, and suspended jobs.


.TH bjobs 1 "June 2023" "" ""
.ll 72

.ce 1000
**-A**
.ce 0


Displays summarized information about job arrays.



# Categories



filter

# Synopsis



**bjobs -A**

# Description



If you specify job arrays with the job array ID, and also specify
-A, do not include the index list with the job array ID.

You can use -w to show the full array specification, if
necessary.

Parent topic: Options


**-a**



Displays information about jobs in all states, including jobs
that finished recently.


# Categories



state


# Synopsis



**bjobs -a**

# Description



The finished jobs that -a displays are those that finished within
an interval specified by CLEAN_PERIOD in lsb.params (the default
period is 1 hour).

Use -a with -x option to display all jobs that have triggered a
job exception (overrun, underrun, idle).

# Examples



bjobs -u all -a

Displays all jobs of all users.

Parent topic: Options


**-aff**


Displays information about jobs with CPU and memory affinity
resource requirements for each task in the job.


# Categories



filter, format
# Synopsis



**bjobs -l** | **-UF** [**-aff**]

# Conflicting Options



Use only with the -l or -UF option.
# Description



If the job is pending, the requested affinity resources are
displayed. For running jobs, the effective and combined affinity
resource allocation decision made by LSF is also displayed, along
with a table headed AFFINITY that shows detailed memory and
CPU binding information for each task, one line for each
allocated processor unit. For finished jobs (EXIT or DONE state),
the affinity requirements for the job, and the effective and
combined affinity resource requirement details are displayed.

Use bhist -l -aff to show the actual affinity resource allocation
for finished jobs.

Parent topic: Options




Displays information about jobs submitted to the specified
application profile.


# Categories



filter

# Synopsis



**bjobs -app** application_profile_name

# Description



You must specify an existing application profile.
# Examples



bjobs -app fluent

Displays all jobs belonging to the application profile
fluent.

Parent topic: Options


**-aps**


Displays absolute priority scheduling (APS) information for
pending jobs in a queue with APS_PRIORITY enabled.


# Categories



format
# Synopsis



**bjobs -aps**
# Description



The APS value is calculated based on the current scheduling
cycle, so jobs are not guaranteed to be dispatched in this order.

Pending jobs are ordered by APS value. Jobs with system APS
values are listed first, from highest to lowest APS value. Jobs
with calculated APS values are listed next ordered from high to
low value. Finally, jobs not in an APS queue are listed. Jobs
with equal APS values are listed in order of submission time. APS
values of jobs not in an APS queue are shown with a dash (-).

If queues are configured with the same priority, bjobs -aps may
not show jobs in the correct expected dispatch order. Jobs may be
dispatched in the order the queues are configured in lsb.queues.
You should avoid configuring queues with the same priority.

For resizable jobs, -aps displays the latest APS information for
running jobs with active resize allocation requests. LSF handles
the dynamic priority for running jobs with active resize
requests. The displayed job priority can change from time to
time.

Parent topic: Options



**-cname**



In IBM Spectrum LSF Advanced Edition, includes the cluster name
for execution cluster hosts in the output.


# Categories



format
# Synopsis



**bjobs -cname**

**Note: **This command option is deprecated and might be
removed in a future version of LSF.

# Examples



% bjobs -l -cname  
Job &lt;1&gt;, User &lt;lsfuser&gt;, Project &lt;default&gt;, Status &lt;RUN&gt;, Queue &lt;queue1&gt;,   
Command &lt;myjob&gt;  
Mon Nov 29 14:08:35: Submitted from host &lt;hostA&gt;, CWD &lt;/home/lsfuser&gt;,   
                     Re-runnable;  
Mon Nov 29 14:08:38: Job &lt;1&gt; forwarded to cluster &lt;cluster3&gt;;  
Mon Nov 29 14:08:44: Started on &lt;hostC@cluster3&gt;, Execution Home   
                     &lt;/home/lsfuser&gt;, Execution CWD &lt;/home/lsfuser&gt;;  
Mon Nov 29 14:08:46: Resource usage collected.  
 MEM: 2 Mbytes;  SWAP: 32 Mbytes;  NTHREAD: 1  
 PGID: 6395;  PIDs: 6395  
   
SCHEDULING PARAMETERS:  
           r15s r1m  r15m ut pg io ls it tmp swp mem  
 loadSched -    -    -    -  -  -  -  -   -  -   -  
 loadStop  -    -    -    -  -  -  -  -   -  -   -  
 ...



Parent topic: Options


**-d**



Displays information about jobs that finished recently.


# Categories



state

# Synopsis



**bjobs -d**

# Description



The finished jobs that -d displays are those that finished within
an interval specified by CLEAN_PERIOD in lsb.params (the default
period is 1 hour).


# Examples



bjobs -d -q short -m hostA -u user1

Displays all the recently finished jobs submitted by user1
to the queue short, and executed on the host hostA.

Parent topic: Options




**-data**



Displays the data file requirements for the job. The -data option
acts as a filter to show only jobs with data requirements. For
example, the option lists jobs that are submitted with -data. The
option also lists files or data requirement tags that are
requested by the job.



# Categories



filter
# Synopsis



**bjobs -data** [jobID]

# Conflicting Options



Do not use the -data option with the following options: -A, -sum.

# Description



The following units are displayed for file size:

*  _nnn_ B if file size is less than 1 KB

*  _nnn_[._n_] KB if file size is less than 1 MB

*  _nnn_[._n_] MB if file size is less than 1 GB

*  _nnn_[._n_] GB if file size is 1 GB or larger

*  _nnn_[._n_] EB if file size is 1 EB or larger

A dash (-) indicates that a size or modification time stamp
value is not available.

For jobs with data requirements that are specified as tags, the
-data option shows the tag names.

# Examples



bjobs -data 1962  
JOBID   USER    STAT  QUEUE  FROM_HOST  EXEC_HOST  JOB_NAME   SUBMIT_TIME  
1962    user1   PEND  normal hostA                 *p 1000000 Sep 20 16:31   
FILE                                 SIZE   MODIFIED   
datahost:/proj/user1/input1.dat      500 M   Jun 27 16:37:52   
datahost:/proj/user1/input2.dat      100 M   Jun 27 16:37:52   
datahost:/proj/user1/input3.dat      -      -  


For jobs with data requirements specified as tags, the -data
option shows the tag names:

bjobs -data 1962  
JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME  
1962    user1   PEND  normal     hostA                   *p 1000000 Sep 20 16:31   
TAG  
OUTPUT_FROM_J1   
OUTPUT_FROM_J2  


For jobs with a folder as data requirement, the -data shows the
folder name. Individual files in the folder are not shown. For
example, for the following data requirement

bsub -data "/home/user/folder1/" myjob

the -data option shows the folder name /home/user/folder1/:

bjobs -data 44843  
JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME  
44843   user1   PEND  normal     hosta                   myjob      May 10 14:42  
  
 FILE                                                 SIZE         MODIFIED      
 hosta:/home/user1/folder1/                           4 KB         May  9 09:53           


Parent topic: Options




**-env**


Displays the environment variables in the job submission
environment for the specified job.



# Categories



filter
# Synopsis



**bjobs -env**

# Conflicting Options



Do not use with any other bjobs command options.
# Description



You must specify a single job ID or job array element ID when
using the -env command option. Multiple job IDs are not
supported.

When using the LSF multicluster capability and a lease job
contains application-specific environment variables, the -env
command option cannot display these application-specific
environment variables when issued from the remote cluster.

Parent topic: Options




**-fwd**


In LSF multicluster capability job forwarding mode, filters
output to display information on forwarded jobs.



# Categories



filter

# Synopsis



**bjobs -fwd**
# Conflicting Options



Do not use with the following options: -A, -d, -sla, -ss, -x.

# Description



In LSF multicluster capability job forwarding mode, filters
output to display information on forwarded jobs, including the
forwarded time and the name of the cluster to which the job was
forwarded. -fwd can be used with other options to further filter
the results. For example, bjobs -fwd -r displays only forwarded
running jobs.

To use -x to see exceptions on the execution cluster, use bjobs
-m _execution\_cluster_ -x.

# Examples



% bjobs -fwd  
JOBID USER    STAT QUEUE  EXEC_HOST JOB_NAME   CLUSTER  FORWARD_TIME  
123   lsfuser RUN  queue1 hostC     sleep 1234 cluster3 Nov 29 14:08  


Parent topic: Options




**-G**


Displays jobs associated with the specified user group.


# Categories



filter

# Synopsis



**bjobs -G** user_group

# Conflicting Options



Do not use with the -u option.

# Description



Only displays jobs associated with a user group submitted with
bsub -G for the specified user group. The –G option does not
display jobs from subgroups within the specified user group. Jobs
associated with the user group at submission are displayed, even
if they are later switched to a different user group.

You can only specify a user group name. The keyword all is
not supported for -G.

Parent topic: Options




**-g**


Displays information about jobs attached to the specified job
group.



# Categories



filter

# Synopsis



**bjobs -g** job_group_name

# Description



Use -g with -sla to display job groups attached to a time-based
service class. Once a job group is attached to a time-based
service class, all jobs submitted to that group are subject to
the SLA.

bjobs -l with -g displays the full path to the group to which a
job is attached.

# Examples



bjobs -g /risk_group  
JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME  
113     user1   PEND  normal     hostA                   myjob     Jun 17 16:15  
111     user2   RUN   normal     hostA       hostA       myjob     Jun 14 15:13  
110     user1   RUN   normal     hostB       hostA       myjob     Jun 12 05:03  
104     user3   RUN   normal     hostA       hostC       myjob     Jun 11 13:18  


To display the full path to the group to which a job is attached,
run bjobs -l -g:

bjobs -l -g /risk_group  
Job &lt;101&gt;, User &lt;user1&gt;, Project &lt;default&gt;, Job Group &lt;/risk_group&gt;,   
    Status &lt;RUN&gt;, Queue &lt;normal&gt;, Command &lt;myjob&gt;  
Tue Jun 17 16:21:49 2009: Submitted from host &lt;hostA&gt;, CWD &lt;/home/user1;  
Tue Jun 17 16:22:01 2009: Started on &lt;hostA&gt;;  
 ...  


Parent topic: Options




**-gpu**



bjobs -l -gpu shows the following information on GPU job
allocation:



# Categories



filter
# Synopsis



**bjobs -l** | **-UF** [**-gpu**]

# Conflicting Options



Use only with the -l or -UF option.
# Description



**Host Name**  
         The name of the host.

**GPU IDs on the host**  
         Each GPU is shown as a separate line.

**TASK and ID**  
         List of job tasks and IDs using the GPU (separated by
         comma if used by multiple tasks)

**MODEL**  
         Contains the GPU brand name and model type name.

**MTOTAL**  
         The total GPU memory size.

**GPU Compute Capability**  


**MRSV**  
         GPU memory reserved by the job

**SOCKET**  
         socket ID of the GPU located at

**NVLINK**  
         Indicates if the GPU has NVLink connections with other
         GPUs allocated for the job (ranked by GPU ID and
         including itself). The connection flag of each GPU is a
         character separated by “/” with the next GPU:  
         A “Y” indicates there is a direct NVLINK connection
         between two GPUs.  
         An “N” shows there is no direct NVLINK connection with
         that GPU.  
         A “-” shows the GPU is itself.

If the job exited abnormally due to a GPU-related error or
warning, the error or warning message displays. If LSF could not
get GPU usage information from DCGM, a hyphen (-) displays.

Parent topic: Options




**Categories**



   Category: filter  
   Filter specific types of jobs: -A, -aff, -app, -aps, -bucket,
   -data, -env, -fwd, -g, -G, -J, -Jd, -Lp, -m, -N, -P, -plan,
   -prio, -psum, -q, -rusage, -script, -sla, -ss, -u.

   Category: format  
   Control the bjobs display format: -aff, -cname, -hostfile, -l,
   -N, -noheader, -o, -psum, -sum, -UF, -w, -W, -WF, -WL, -WP,
   -X.

   Category: state  
   Display specific job states: -a, -d, -N, -p, -pe, -pei, -pi,
   -psum, -r, -s, -sum, -x.

Parent topic: bjobs




**Options**



List of options for the bjobs command.

   -A  
   Displays summarized information about job arrays.

   -a  
   Displays information about jobs in all states, including jobs
   that finished recently.

   -aff  
   Displays information about jobs with CPU and memory affinity
   resource requirements for each task in the job.

   -app  
   Displays information about jobs submitted to the specified
   application profile.

   -aps  
   Displays absolute priority scheduling (APS) information for
   pending jobs in a queue with APS_PRIORITY enabled.

   -cname  
   In IBM Spectrum LSF Advanced Edition, includes the cluster
   name for execution cluster hosts in the output.

   -d  
   Displays information about jobs that finished recently.

   -data  
   Displays the data file requirements for the job. The -data
   option acts as a filter to show only jobs with data
   requirements. For example, the option lists jobs that are
   submitted with -data. The option also lists files or data
   requirement tags that are requested by the job.

   -env  
   Displays the environment variables in the job submission
   environment for the specified job.

   -fwd  
   In LSF multicluster capability job forwarding mode, filters
   output to display information on forwarded jobs.

   -G  
   Displays jobs associated with the specified user group.

   -g  
   Displays information about jobs attached to the specified job
   group.

   -gpu  
   bjobs -l -gpu shows the following information on GPU job
   allocation:

   -hms  
   Displays times in the customized output in _hh:mm:ss_
   format.

   -hostfile  
   Displays information about a job submitted with a
   user-specified host file.

   -J  
   Displays information about jobs or job arrays with the
   specified job name.

   -Jd  
   Displays information about jobs with the specified job
   description.

   -json  
   Displays the customized output in JSON format.

   -Lp  
   Displays jobs that belong to the specified LSF License
   Scheduler project.

   -l  
   Long format. Displays detailed information for each job in a
   multi-line format.

   -m  
   Displays jobs dispatched to the specified hosts.

   -N  
   Displays information about done and exited jobs, also displays
   the normalized CPU time consumed by the job.

   -noheader  
   Removes the column headings from the output.

   -o  
   Sets the customized output format.

   -P  
   Displays jobs that belong to the specified project.

   -p  
   Displays pending jobs, together with the pending reasons that
   caused each job not to be dispatched during the last dispatch
   turn.

   -pe  
   Displays pending jobs that are eligible for scheduling.

   -pei  
   Displays pending jobs divided into lists of jobs that are
   eligible for scheduling and ineligible for scheduling.

   -pi  
   Displays pending jobs that are ineligible for scheduling.

   -plan  
   Filter for the PEND jobs that have an allocation plan. See
   **ALLOCATION\_PLANNER** (lsb.params).

   -prio  
   Displays the detailed absolute priority scheduling (APS)
   factor values for all pending jobs.

   -psum  
   Displays a summarized version of reasons for pending jobs.

   -q  
   Displays jobs in the specified queue.

   -r  
   Displays running jobs.

   -rusage  
   Displays jobs requesting the resources specified by the
   filter.

   -s  
   Displays suspended jobs, together with the suspending reason
   that caused each job to become suspended.

   -script  
   Displays the job script for the specified job from the LSF
   info directory.

   -sla  
   Displays jobs belonging to the specified service class.

   -ss  
   Displays summary information for LSF Session Scheduler tasks.

   -sum  
   Displays summary information about unfinished jobs.

   -U  
   Displays jobs that are associated with the specified advance
   reservation.

   -UF  
   Displays unformatted job detail information.

   -u  
   Displays jobs that were submitted by the specified users or
   user groups.

   -W  
   Provides resource usage information for: PROJ\_NAME,
   CPU\_USED, MEM, SWAP, PIDS,
   START\_TIME, FINISH\_TIME.

   -WF  
   Displays an estimated finish time for running or pending jobs.
   For done or exited jobs, displays the actual finish time.

   -WL  
   Displays the estimated remaining run time of jobs.

   -WP  
   Displays the current estimated completion percentage of jobs.

   -w  
   Wide format. Displays job information without truncating
   fields.

   -X  
   Displays noncondensed output for host groups and compute
   units.

   -x  
   Displays unfinished jobs that have triggered a job exception
   (overrun, underrun, idle,
   runtime\_est\_exceeded).

   job_id  
   Specifies the jobs or job arrays that bjobs displays.

   -h  
   Displays a description of the specified category, command
   option, or sub-option to stderr and exits.

   -V  
   Prints LSF release version to stderr and exits.

Parent topic: bjobs



.TH bjobs 1 "June 2023" "" ""
.ll 72

.ce 1000
**-hms**
.ce 0


Displays times in the customized output in _hh:mm:ss_ format.


# Categories



format

# Synopsis



**bjobs -o** format [**-json**] **-hms**


# Conflicting Options



Use only with the -o and -o -json options.

# Description



When specified, bjobs displays any times (but not time stamps) in
the customized output in _hh:mm:ss_ format.

The cpu\_used field normally uses one decimal place in the
bjobs output, but if you specify -hms, bjobs rounds up this field
to the next second. For example, if the cpu\_used field is
0.2 seconds, specifying -hms rounds this number up to
00:00:01.

For the runtimelimit field, if the **ABS\_RUNLIMIT**
parameter is defined as Y in the lsb.params file and you
specify -hms, bjobs does not display the host name. That is, if
ABS\_RUNLIMIT=Y is defined in the lsb.params file, bjobs
normally displays the host name after the runtime limit (for
example, 3.0/hostA). If you specify -hms, bjobs displays
the runtime limit without the host name (for example,
00:03:00). This also applies if the **ABS\_RUNLIMIT**
parameter is defined as Y in the lsb.applications file and
you specify -hms to a job that is submitted to an application
profile with ABS\_RUNLIMIT=Y.

Specifying the -hms option overrides the
**LSB\_HMS\_TIME\_FORMAT** environment variable, which overrides
the **LSB\_HMS\_TIME\_FORMAT** parameter setting in the lsf.conf
file.

This option applies only to output for the bjobs -o and bjobs -o
-json commands for customized output. This option has no effect
when run with bjobs without the -o option.

Parent topic: Options




**-hostfile**



Displays information about a job submitted with a user-specified
host file.



# Categories



format

# Synopsis



**bjobs -l** | **-UF** [**-hostfile**]

# Conflicting Options



Use only with the -l or -UF option.

# Description



If a job was submitted with bsub -hostfile or modified with bmod
-hostfile to point to a user-specified host file, use -hostfile
to show the user-specified host file path as well as the contents
of the host file.

Use -hostfile together with -l or -UF, to view the user specified
host file content as well as the host allocation for a given job.

# Example



Use -l -hostfile to display a user-specified host file that was
submitted with a job or added to a job.

For example:

bjobs -l -hostfile  2012  
Job &lt;2012&gt;, User &lt;userG&gt;, Project &lt;myproject&gt;, Status &lt;PEND&gt;, Queue   
                         &lt;normal&gt;, Commnad &lt;sleep 10000&gt;  
                         Thu Aug 1 12:43:25: Submitted from host &lt;host10a&gt;,  
                         CWD &lt;$HOME&gt;,Host file &lt;/home/userG/myhostfile&gt;;  
  
 ......

USER-SPECIFIED HOST FILE:  
HOST            SLOTS  
host01            3  
host02            1  
host01            1  
host02            2  
host03            1  


Parent topic: Options




**-J**


Displays information about jobs or job arrays with the specified
job name.



# Categories



filter


# Synopsis



**bjobs -J** job_name

# Description



Only displays jobs that were submitted by the user running this
command.

The job name can be up to 4094 characters long. Job names are not
unique.

The wildcard character (*) can be used anywhere within a job
name, but cannot appear within array indices. For example
job* returns jobA and jobarray[1], *AAA*[1] returns
the first element in all job arrays with names containing AAA,
however job1[*] will not return anything since the wildcard
is within the array index.

Parent topic: Options




**-Jd**


Displays information about jobs with the specified job
description.



# Categories



filter


# Synopsis



**bjobs -Jd** job_description

# Description



Only displays jobs that were submitted by the user running this
command.

The job description can be up to 4094 characters long. Job
descriptions are not unique.

The wildcard character (*) can be used anywhere within a job
description.

Parent topic: Options




**-json**



Displays the customized output in JSON format.



# Categories



format

# Synopsis



**bjobs -o** format **-json**

# Conflicting Options



Do not use with the -noheader option.

# Description



**Note: **The -json command is not supported for Microsoft
Windows or Apple OS X operating systems.

When specified, bjobs -o displays the customized output in the
JSON format.

This option applies only to output for the bjobs -o command for
customized output. This option has no effect when run with bjobs
without the -o option and the **LSB\_BJOBS\_FORMAT** environment
variable or parameter are not defined.

Parent topic: Options



**-l**


Long format. Displays detailed information for each job in a
multi-line format.



# Categories



format
# Synopsis



**bjobs -l**

# Description



The -l option displays the following additional information:

*  Project name

*  Job command

*  Current working directory on the submission host

*  Initial checkpoint period

*  Checkpoint directory

*  Migration threshold

*  Predicted job start time

*  Pending and suspending reasons

*  Job status

*  Job kill reason

*  Number of resources (or, starting in Fix Pack 14, the assigned
   resource names for each job)

*  Resource usage

*  Resource usage limits information

*  Memory and CPU usage information, such as CPU efficiency, CPU
   peak usage, and memory efficiency values

*  Runtime resource usage information on the execution hosts

*  Pending time limits, and eligible pending time limits

*  Job description

*  Name of any esub or epsub used with the job

*  Energy usage (if energy accounting with IBM Spectrum LSF
   Explorer is enabled by setting
   LSF\_QUERY\_ES\_FUNCTIONS="energy" or "all" in the
   lsf.conf file)

*  Approximate accumulated job disk usage (I/O) data on IBM
   Spectrum Scale (if IBM Spectrum Scale I/O accounting with IBM
   Spectrum LSF Explorer is enabled by setting
   LSF\_QUERY\_ES\_FUNCTIONS="gpfsio" or "all" in the
   lsf.conf file)

*  Planned start time for jobs with a schedule and reservation
   plan.

*  GPU requirement and allocation information

*  Account name for LSF resource connector

If the job was submitted with the bsub -K command, the -l option
displays Synchronous Execution.

Use the bjobs -A -l command to display detailed information for
job arrays, including job array job limit (%
_job\_limit_) if set.

Use the bjobs -ss -l command to display detailed information for
session scheduler jobs.

Use the bjobs -data -l command to display detailed information
for jobs with data requirements (for example, jobs that are
submitted with -data).

The bjobs -pl command displays detailed information about
all pending jobs of the invoker.

If the **JOB\_IDLE** parameter is configured in the queue, use
bjobs -l to display job idle exception information.

If you submitted your job with the -U option to use advance
reservations that are created with the brsvadd command, bjobs -l
shows the reservation ID used by the job.

If the **LSF\_HPC\_EXTENSIONS="SHORT\_PIDLIST"** parameter is
specified in the lsf.conf file, the output from bjobs is
shortened to display only the first PID and a count of the
process group IDs (PGIDs) and process IDs for the job. Without
**SHORT\_PIDLIST**, all of the process IDs (PIDs) for a job are
displayed.

If the **LSF\_HPC\_EXTENSIONS="HOST\_RUSAGE"** parameter is
specified in the lsf.conf file, the output from the bjobs -l
command reports the correct rusage-based usage and the total
rusage that is being charged to the execution host.

If you submitted a job with multiple resource requirement strings
by using the bsub -R option for the order, same, rusage, and
select sections, bjobs -l displays a single, merged resource
requirement string for those sections, as if they were submitted
by using a single -R option.

If you submitted a job by using the OR (||) expression to specify
alternative resources, this option displays the Execution rusage
string with which the job runs.

Predicted start time for PEND reserve job is not shown with
the bjobs -l command option. LSF does not calculate the predicted
start time for PEND reserve job if no back fill queue is
configured in the system. In that case, resource reservation for
PEND jobs works as normal, and no predicted start time is
calculated.

For resizable jobs, the -l option displays active pending resize
allocation requests, and the latest job priority for running jobs
with active pending resize requests.

For jobs with user-based fair share scheduling, displays the
charging SAAP (share attribute account path).

For jobs submitted to an absolute priority scheduling (APS)
queue, -l shows the ADMIN factor value and the system APS
value if they are not set by the administrator for the job.

For jobs submitted with SSH X11 forwarding, displays that the job
was submitted in SSH X11 forwarding mode as well as the SSH
command submitted (set in **LSB\_SSH\_XFORWARD\_CMD** in
lsf.conf).

If the job was auto-attached to a guarantee SLA, -l displays the
auto-attached SLA name.

Specified CWD shows the value of the bsub -cwd option or the
value of **LSB\_JOB\_CWD**. The CWD path with pattern values is
displayed. CWD is the submission directory where bsub ran. If
specified CWD was not defined, this field is not shown. The
execution CWD with pattern values is always shown.

If the job was submitted with an energy policy, to automatically
select a CPU frequency, -l shows the Combined CPU frequency (the
CPU frequency that is selected for the job based on the energy
policy tag, energy policy, and threshold file). If the job was
submitted with a user-defined CPU frequency (by using bsub
-freq), -l shows the Specified CPU frequency for the job.

For jobs submitted with the default GPU requirements (with the
option -gpu -), use the bjobs -l command to see the default
job-level resource requirement without details like
&lt;num=1...&gt;: Requested GPU.

If the -gpu option specifies GPU requirements (for example,
-gpu num=3, the bjobs -l shows the details as Requested
GPU &lt;num=3&gt;.

The bjobs -l command displays an output section for GPU jobs that
shows the combined and effective GPU requirements that are
specified for the job. The GPU requirement string is in the
format of the **GPU\_REQ** parameter in the application profile
or queue:

*  The combined GPU requirement is merged based on the current
   GPU requirements in job, queue, application, or default GPU
   requirement.

*  The effective GPU requirement is the one used by a started
   job. It never changes after a job is started.

Jobs that are submitted with an esub (or epsub) by using bsub -a
(or modified by using bmod -a), shows the latest esubs used for
execution in bjobs -l output, first with the default and then
user esubs. If a user-specified esub script is the same as the
default esub script, the duplicate esubs shows as one entry. If a
job is submitted with an esub containing parameters, the esub and
its parameters are shown in bjobs -l as well, and the format of
the esub is the same as the esub that is specified in the job
submission. For example:

bsub -a "test(a,b,c) " sleep 10000

is shown as:

Job &lt;1561&gt;, User &lt;joes&gt;, Project &lt;default&gt;, Status &lt;RUN&gt;, Queue &lt;normal&gt;,   
Command &lt;sleep 1000000&gt;, Share group charged &lt;/joes&gt;, Esub &lt;test(a,b,c)&gt;

If enhanced energy accounting with IBM Spectrum LSF Explorer is
enabled (with **LSF\_ENABLE\_BEAT\_SERVICE** in lsf.conf), output
shows the energy usage in Joule and kWh.

If the lsb.params configuration file is configured with
ALLOCATION_PLANNER = Y, and sets of candidate jobs have
been identified for consideration for an allocation plan, LSF
creates a scheduling and reservation allocation plan.

*  bjobs –l : displays the planned start time for all jobs with
   an allocation plan.

*  bjobs –l –plan : filter for jobs with allocation plans,
   displaying the planned start time and planned allocation for
   each job.

Parent topic: Options




**-Lp**



Displays jobs that belong to the specified LSF License Scheduler
project.



# Categories



filter

# Synopsis



**bjobs -Lp** ls_project_name

Parent topic: Options




**-m**

Displays jobs dispatched to the specified hosts.



# Categories



filter

# Synopsis



**bjobs -m** host_name ... | **-m** host_group ... | **-m**
cluster_name

# Description



To see the available hosts, use bhosts.

If a host group or compute unit is specified, displays jobs
dispatched to all hosts in the group. To determine the available
host groups, use bmgroup. To determine the available compute
units, use bmgroup -cu.

With LSF multicluster capability, displays jobs in the specified
cluster. You can specify a single cluster. If a remote cluster
name is specified, you see the remote job ID, even if the
execution host belongs to the local cluster. To determine the
available clusters, use bclusters.

# Examples



bjobs -d -q short -m hostA -u user1

Displays all the recently finished jobs submitted by user1
to the queue short, and executed on the host hostA.

Parent topic: Options



**-N**


Displays information about done and exited jobs, also displays
the normalized CPU time consumed by the job.



# Categories



filter, format, state

# Synopsis



**bjobs -N** host_name | **-N** host_model | **-N**
cpu_factor

# Description



Normalizes using the CPU factor specified, or the CPU factor of
the host or host model specified.

Use with -p, -r, and -s to show information about pending,
running, and suspended jobs along with done and exited jobs.

Parent topic: Options




**-noheader**


Removes the column headings from the output.



# Categories



format


# Synopsis



**bjobs -noheader**

# Description



When specified, bjobs displays the values of the fields without
displaying the names of the fields. This is useful for script
parsing, when column headings are not necessary.

This option applies to output for the bjobs command with no
options, and to output for all bjobs options with short form
output except for -aff, -l, -UF, -N, -h, and -V.

Parent topic: Options



**-o**


Sets the customized output format.



# Categories



format

# Synopsis



**bjobs -o "**[field_name | **all**][**:**[**-**]
[output\_width]][**:**unit\_prefix]...[**delimiter='**character**'**]**"**

**bjobs -o '**[field_name | **all**][**:**[**-**]
[output\_width]][**:**unit_prefix] ...
[**delimiter="**character**"**]**'**

# Description



*  Specify which bjobs fields (or aliases instead of the full
   field names), in which order, and with what width to display.

*  Specify only the bjobs field name or alias to set its output
   to unlimited width and left justification.

*  (Available starting in Fix Pack 14) Specify all to
   display all fields. Specify the colon (:) with an output
   width that applies to all fields.

*  Specify the colon (:) without an output width to set the
   output width to the recommended width for that field.

*  Specify the colon (:) with an output width to set the
   maximum number of characters to display for the field. When
   its value exceeds this width, bjobs truncates the output:

   *  For the JOB_NAME field, bjobs removes the header characters
      and replaces them with an asterisk (*)

   *  For other fields, bjobs truncates the ending characters

*  Specify a hyphen (-) to set right justification when
   bjobs displays the output for the specific field. If not
   specified, the default is to set left justification when bjobs
   displays the output for a field.

*  Specify a second colon (:) with a unit to specify a unit
   prefix for the output for the following fields: mem,
   max\_mem, avg\_mem, memlimit, swap,
   swaplimit, corelimit, stacklimit, and
   hrusage (for hrusage, the unit prefix is for
   mem and swap resources only).

   This unit is KB (or K) for kilobytes, MB (or
   M) for megabytes, GB (or G) for gigabytes,
   TB (or T) for terabytes, PB (or P) for
   petabytes, EB (or E) for exabytes, ZB (or
   Z) for zettabytes), or S to automatically adjust
   the value to a suitable unit prefix and remove the "bytes"
   suffix from the unit. The default is to automatically adjust
   the value to a suitable unit prefix, but keep the "bytes"
   suffix in the unit.

   The display value keeps two decimals but rounds up the third
   decimal. For example, if the unit prefix is set to G,
   10M displays as 0.01G.

   The unit prefix specified here overrides the value of the
   **LSB\_UNIT\_FOR\_JOBS\_DISPLAY** environment variable, which
   also overrides the value of the
   **LSB\_UNIT\_FOR\_JOBS\_DISPLAY** parameter in the lsf.conf
   file.

*  Use delimiter= to set the delimiting character to
   display between different headers and fields. This delimiter
   must be a single character. By default, the delimiter is a
   space.

To specify special delimiter characters in a csh environment (for
example, $), use double quotation marks (") in the
delimiter specification and single quotation marks (') in
the -o statement:

bjobs ... -o
'_field\_name_[:[-][_output\_width_]]
 ... [delimiter="_character_"]'

The -o option applies only to output for certain bjobs options:

*  This option applies to output for the bjobs command with no
   options, and for bjobs options with short form output that
   filter information, including the following options: -a, -app,
   -d, -g, -G, -J, -Jd, -Lp, -m, -P, -q, -r, -sla, -u, -x, -X.

*  This option applies to output for bjobs options that use a
   modified format and filter information, including the
   following options: -fwd, -N, -p, -s.

*  This option does not apply to output for bjobs options that
   use a modified format, including the following options: -A,
   -aff, -aps, -l, -UF, -ss, -sum, -UF, -w, -W, -WF, -WL, -WP.

The bjobs -o option overrides the **LSB\_BJOBS\_FORMAT**
environment variable, which overrides the **LSB\_BJOBS\_FORMAT**
setting in lsf.conf.

The following are the field names used to specify the bjobs
fields to display, recommended width, aliases you can use instead
of field names, and units of measurement for the displayed field:

**Table 1. Output fields for bjobs**



+------------------------+------+------------+-----------------+
| Field name             | Widt | Aliases    | Unit            |
|                        | h    |            |                 |
+------------------------+------+------------+-----------------+
| jobid                  | 7    | id         |                 |
+------------------------+------+------------+-----------------+
| jobindex               | 8    |            |                 |
+------------------------+------+------------+-----------------+
| stat                   | 5    |            |                 |
+------------------------+------+------------+-----------------+
| user                   | 7    |            |                 |
+------------------------+------+------------+-----------------+
| user_group             | 15   | ugroup     |                 |
+------------------------+------+------------+-----------------+
| queue                  | 10   |            |                 |
+------------------------+------+------------+-----------------+
| job_name               | 10   | name       |                 |
+------------------------+------+------------+-----------------+
| job_description        | 17   | descriptio |                 |
|                        |      | n          |                 |
+------------------------+------+------------+-----------------+
| proj_name              | 11   | proj,      |                 |
|                        |      | project    |                 |
+------------------------+------+------------+-----------------+
| application            | 13   | app        |                 |
+------------------------+------+------------+-----------------+
| service_class          | 13   | sla        |                 |
+------------------------+------+------------+-----------------+
| job_group              | 10   | group      |                 |
+------------------------+------+------------+-----------------+
| job_priority           | 12   | priority   |                 |
+------------------------+------+------------+-----------------+
| rsvid                  | 40   |            |                 |
+------------------------+------+------------+-----------------+
| esub                   | 20   |            |                 |
+------------------------+------+------------+-----------------+
| kill_reason            | 50   |            |                 |
+------------------------+------+------------+-----------------+
| suspend_reason         | 50   |            |                 |
+------------------------+------+------------+-----------------+
| resume_reason          | 50   |            |                 |
+------------------------+------+------------+-----------------+
| kill_issue_host        | 50   |            |                 |
+------------------------+------+------------+-----------------+
| suspend_issue_host     | 50   |            |                 |
+------------------------+------+------------+-----------------+
| resume_issue_host      | 50   |            |                 |
+------------------------+------+------------+-----------------+
| dependency             | 15   |            |                 |
+------------------------+------+------------+-----------------+
| pend_reason            | 11   |            |                 |
| This displays the      |      |            |                 |
| single key pending     |      |            |                 |
| reason, including      |      |            |                 |
| custom messages,       |      |            |                 |
| regardless of the      |      |            |                 |
| default pending reason |      |            |                 |
| level as specified in  |      |            |                 |
| the                    |      |            |                 |
| LSB_BJOBS_PENDREASON_L |      |            |                 |
| EVEL parameter in the  |      |            |                 |
| lsf.conf file.         |      |            |                 |
+------------------------+------+------------+-----------------+
| charged_saap           | 50   | saap       |                 |
+------------------------+------+------------+-----------------+
| command                | 15   | cmd        |                 |
+------------------------+------+------------+-----------------+
| pre_exec_command       | 16   | pre_cmd    |                 |
+------------------------+------+------------+-----------------+
| post_exec_command      | 17   | post_cmd   |                 |
+------------------------+------+------------+-----------------+
| resize_notification_co | 27   | resize_cmd |                 |
| mmand                  |      |            |                 |
+------------------------+------+------------+-----------------+
| pids                   | 20   |            |                 |
+------------------------+------+------------+-----------------+
| exit_code              | 10   |            |                 |
+------------------------+------+------------+-----------------+
| exit_reason            | 50   |            |                 |
+------------------------+------+------------+-----------------+
| interactive            | 11   |            |                 |
+------------------------+------+------------+-----------------+
| from_host              | 11   |            |                 |
+------------------------+------+------------+-----------------+
| first_host             | 11   |            |                 |
+------------------------+------+------------+-----------------+
| exec_host              | 11   |            |                 |
+------------------------+------+------------+-----------------+
| nexec_host             | 10   |            |                 |
| Note: If the allocated |      |            |                 |
| host group or compute  |      |            |                 |
| unit is condensed,     |      |            |                 |
| this field does not    |      |            |                 |
| display the real       |      |            |                 |
| number of hosts. Use   |      |            |                 |
| bjobs -X -o to view    |      |            |                 |
| the real number of     |      |            |                 |
| hosts in these         |      |            |                 |
| situations.            |      |            |                 |
+------------------------+------+------------+-----------------+
| ask_hosts              | 30   |            |                 |
+------------------------+------+------------+-----------------+
| alloc_slot             | 20   |            |                 |
+------------------------+------+------------+-----------------+
| nalloc_slot            | 10   |            |                 |
+------------------------+------+------------+-----------------+
| host_file              | 10   |            |                 |
+------------------------+------+------------+-----------------+
| exclusive              | 13   |            |                 |
+------------------------+------+------------+-----------------+
| nreq_slot              | 10   |            |                 |
+------------------------+------+------------+-----------------+
| submit_time            | 15   |            | time stamp      |
+------------------------+------+------------+-----------------+
| start_time             | 15   |            | time stamp      |
+------------------------+------+------------+-----------------+
| estimated_start_time   | 20   | estart_tim | time stamp      |
|                        |      | e          |                 |
+------------------------+------+------------+-----------------+
| specified_start_time   | 20   | sstart_tim | time stamp      |
|                        |      | e          |                 |
+------------------------+------+------------+-----------------+
| specified_terminate_ti | 24   | sterminate | time stamp      |
| me                     |      | _time      |                 |
+------------------------+------+------------+-----------------+
| time_left              | 11   |            | seconds         |
+------------------------+------+------------+-----------------+
| finish_time            | 16   |            | time stamp      |
+------------------------+------+------------+-----------------+
| estimated_run_time     | 20   | ertime     | seconds         |
+------------------------+------+------------+-----------------+
| ru_utime               | 12   |            | seconds         |
+------------------------+------+------------+-----------------+
| ru_stime               | 12   |            | seconds         |
+------------------------+------+------------+-----------------+
| %complete              | 11   |            |                 |
+------------------------+------+------------+-----------------+
| warning_action         | 15   | warn_act   |                 |
+------------------------+------+------------+-----------------+
| action_warning_time    | 19   | warn_time  |                 |
+------------------------+------+------------+-----------------+
| pendstate              | 9    |            |                 |
| (IPEND/EPEND/NOTPEND)  |      |            |                 |
+------------------------+------+------------+-----------------+
| pend_time              | 12   |            | seconds         |
+------------------------+------+------------+-----------------+
| ependtime              | 12   |            | seconds         |
+------------------------+------+------------+-----------------+
| ipendtime              | 12   |            | seconds         |
+------------------------+------+------------+-----------------+
| estimated_sim_start_ti | 24   | esstart_ti | time stamp      |
| me                     |      | me         |                 |
+------------------------+------+------------+-----------------+
| effective_plimit (run  | 18   |            | seconds         |
| with bjobs -p to show  |      |            |                 |
| information for        |      |            |                 |
| pending jobs only)     |      |            |                 |
+------------------------+------+------------+-----------------+
| plimit_remain (run     | 15   |            | seconds         |
| with bjobs -p to show  |      |            |                 |
| information for        |      |            |                 |
| pending jobs only)     |      |            |                 |
| A negative number      |      |            |                 |
| indicates the amount   |      |            |                 |
| of time in which the   |      |            |                 |
| job exceeded the       |      |            |                 |
| pending time limit,    |      |            |                 |
| while a positive       |      |            |                 |
| number shows that the  |      |            |                 |
| time remaining until   |      |            |                 |
| the pending time limit |      |            |                 |
| is reached.            |      |            |                 |
+------------------------+------+------------+-----------------+
| effective_eplimit (run | 19   |            | seconds         |
| with bjobs -p to show  |      |            |                 |
| information for        |      |            |                 |
| pending jobs only)     |      |            |                 |
+------------------------+------+------------+-----------------+
| eplimit_remain (run    | 16   |            | seconds         |
| with bjobs -p to show  |      |            |                 |
| information for        |      |            |                 |
| pending jobs only)     |      |            |                 |
+------------------------+------+------------+-----------------+
| cpu_used               | 10   |            |                 |
| A negative number      |      |            |                 |
| indicates the amount   |      |            |                 |
| of time in which the   |      |            |                 |
| job exceeded the       |      |            |                 |
| pending time limit,    |      |            |                 |
| while a positive       |      |            |                 |
| number shows that the  |      |            |                 |
| time remaining until   |      |            |                 |
| the pending time limit |      |            |                 |
| is reached.            |      |            |                 |
+------------------------+------+------------+-----------------+
| run_time               | 15   |            | seconds         |
+------------------------+------+------------+-----------------+
| idle_factor            | 11   |            |                 |
+------------------------+------+------------+-----------------+
| exception_status       | 16   | except_sta |                 |
|                        |      | t          |                 |
+------------------------+------+------------+-----------------+
| slots                  | 5    |            |                 |
+------------------------+------+------------+-----------------+
| mem                    | 10   |            | LSF_UNIT_FOR_LI |
|                        |      |            | MITS in         |
|                        |      |            | lsf.conf (KB by |
|                        |      |            | default)        |
+------------------------+------+------------+-----------------+
| max_mem                | 10   |            | LSF_UNIT_FOR_LI |
|                        |      |            | MITS in         |
|                        |      |            | lsf.conf (KB by |
|                        |      |            | default)        |
+------------------------+------+------------+-----------------+
| avg_mem                | 10   |            | LSF_UNIT_FOR_LI |
|                        |      |            | MITS in         |
|                        |      |            | lsf.conf (KB by |
|                        |      |            | default)        |
+------------------------+------+------------+-----------------+
| memlimit               | 10   |            | LSF_UNIT_FOR_LI |
|                        |      |            | MITS in         |
|                        |      |            | lsf.conf (KB by |
|                        |      |            | default)        |
+------------------------+------+------------+-----------------+
| swap                   | 10   |            | LSF_UNIT_FOR_LI |
|                        |      |            | MITS in         |
|                        |      |            | lsf.conf (KB by |
|                        |      |            | default)        |
+------------------------+------+------------+-----------------+
| swaplimit              | 10   |            | LSF_UNIT_FOR_LI |
|                        |      |            | MITS in         |
|                        |      |            | lsf.conf (KB by |
|                        |      |            | default)        |
+------------------------+------+------------+-----------------+
| gpu_num                | 10   | gnum       |                 |
+------------------------+------+------------+-----------------+
| gpu_mode               | 20   | gmode      |                 |
+------------------------+------+------------+-----------------+
| j_exclusive            | 15   | j_excl     |                 |
+------------------------+------+------------+-----------------+
| gpu_alloc              | 30   | galloc     |                 |
+------------------------+------+------------+-----------------+
| nthreads               | 10   |            |                 |
+------------------------+------+------------+-----------------+
| hrusage                | 50   |            |                 |
+------------------------+------+------------+-----------------+
| min_req_proc           | 12   |            |                 |
+------------------------+------+------------+-----------------+
| max_req_proc           | 12   |            |                 |
+------------------------+------+------------+-----------------+
| effective_resreq       | 17   | eresreq    |                 |
+------------------------+------+------------+-----------------+
| combined_resreq        | 20   | cresreq    |                 |
+------------------------+------+------------+-----------------+
| network_req            | 15   |            |                 |
+------------------------+------+------------+-----------------+
| filelimit              | 10   |            |                 |
+------------------------+------+------------+-----------------+
| corelimit              | 10   |            |                 |
+------------------------+------+------------+-----------------+
| stacklimit             | 10   |            |                 |
+------------------------+------+------------+-----------------+
| processlimit           | 12   |            |                 |
+------------------------+------+------------+-----------------+
| runtimelimit           | 12   |            |                 |
+------------------------+------+------------+-----------------+
| plimit                 | 10   |            | seconds         |
+------------------------+------+------------+-----------------+
| eplimit                | 10   |            | seconds         |
+------------------------+------+------------+-----------------+
| input_file             | 10   |            |                 |
+------------------------+------+------------+-----------------+
| output_file            | 11   |            |                 |
+------------------------+------+------------+-----------------+
| error_file             | 10   |            |                 |
+------------------------+------+------------+-----------------+
| output_dir             | 15   |            |                 |
+------------------------+------+------------+-----------------+
| sub_cwd                | 10   |            |                 |
+------------------------+------+------------+-----------------+
| exec_home              | 10   |            |                 |
+------------------------+------+------------+-----------------+
| exec_cwd               | 10   |            |                 |
+------------------------+------+------------+-----------------+
| licproject             | 20   |            |                 |
+------------------------+------+------------+-----------------+
| forward_cluster        | 15   | fwd_cluste |                 |
|                        |      | r          |                 |
+------------------------+------+------------+-----------------+
| forward_time           | 15   | fwd_time   | time stamp      |
+------------------------+------+------------+-----------------+
| srcjobid               | 8    |            |                 |
+------------------------+------+------------+-----------------+
| dstjobid               | 8    |            |                 |
+------------------------+------+------------+-----------------+
| source_cluster         | 15   | srcluster  |                 |
+------------------------+------+------------+-----------------+
| energy                 |      |            | Joule           |
+------------------------+------+------------+-----------------+
| gpfsio                 |      |            |                 |
| Job disk usage (I/O)   |      |            |                 |
| data on IBM Spectrum   |      |            |                 |
| Scale.                 |      |            |                 |
+------------------------+------+------------+-----------------+
| cpu_peak               | 10   |            |                 |
|                        |      |            |                 |
| (Available starting in |      |            |                 |
| Fix Pack 13)           |      |            |                 |
|                        |      |            |                 |
+------------------------+------+------------+-----------------+
| cpu_efficiency (for    | 10   |            |                 |
| Fix Pack 13)           |      |            |                 |
|                        |      |            |                 |
| cpu_peak_efficiency    |      |            |                 |
| (for Fix Pack 14)      |      |            |                 |
+------------------------+------+------------+-----------------+
| mem_efficiency         | 10   |            |                 |
|                        |      |            |                 |
| (Available starting in |      |            |                 |
| Fix Pack 13)           |      |            |                 |
+------------------------+------+------------+-----------------+
| average_cpu_efficiency | 10   |            |                 |
| (Available starting in |      |            |                 |
| Fix Pack 14)           |      |            |                 |
+------------------------+------+------------+-----------------+
| cpu_peak_reached_durat | 10   |            |                 |
| ion                    |      |            |                 |
| (Available starting in |      |            |                 |
| Fix Pack 14)           |      |            |                 |
+------------------------+------+------------+-----------------+
| all                    | Spec |            |                 |
| (Available starting in | ify  |            |                 |
| Fix Pack 14)           | an   |            |                 |
|                        | outp |            |                 |
|                        | ut   |            |                 |
|                        | widt |            |                 |
|                        | h    |            |                 |
|                        | that |            |                 |
|                        | appl |            |                 |
|                        | ies  |            |                 |
|                        | to   |            |                 |
|                        | all  |            |                 |
|                        | fiel |            |                 |
|                        | ds   |            |                 |
+------------------------+------+------------+-----------------+

Field names and aliases are not case-sensitive. Valid values for
the output width are any positive integer 1 - 4096. If the
jobid field is defined with no output width and
**LSB\_JOBID\_DISP\_LENGTH** is defined in lsf.conf, the
**LSB\_JOBID\_DISP\_LENGTH** value is used for the output width.
If jobid is defined with a specified output width, the
specified output width overrides the **LSB\_JOBID\_DISP\_LENGTH**
value.

# Example



bjobs -o "jobid stat: queue:- project:10 application:-6
mem:12:G delimiter='^'" 123

This command (used to illustrate the different subcommands for
-o) displays the following fields for a job with the job ID 123:

*  JOBID with unlimited width and left-aligned. If
   LSB_JOBID_DISP_LENGTH is specified, that value is used for the
   output width instead.

*  STAT with a maximum width of 5 characters (which is the
   recommended width) and left-aligned.

*  QUEUE with a maximum width of 10 characters (which is the
   recommended width) and right-aligned.

*  PROJECT with a maximum width of 10 characters and
   left-aligned.

*  APPLICATION with a maximum width of 6 characters and
   right-aligned.

*  MEM with a maximum width of 12 characters, a unit prefix of G
   (for gigabytes), and left-aligned.

*  The ^ character is displayed between different headers
   and fields.

Parent topic: Options




**-P**



Displays jobs that belong to the specified project.




# Categories



filter

<a name="synopsis"></a>

# Synopsis



**bjobs -P** project_name

Parent topic: Options




**-p**



Displays pending jobs, together with the pending reasons that
caused each job not to be dispatched during the last dispatch
turn.



# Categories



state

# Synopsis



**bjobs -p**

# Description



Displays the pending reason or reasons and the number of hosts
giving that reason.

*  0: Displays bjobs -p output as before the LSF 10.1 release.

*  1: Displays the single key pending reason.

*  2: Displays categorized host-based pending reasons for
   candidate hosts in the cluster. For the candidate hosts, the
   actual reason on each host is shown. For each pending reason,
   the number of hosts that give the reason is shown. The actual
   pending reason messages appear from most to least common.

*  3: Displays categorized host-based pending reasons for both
   candidate and non-candidate hosts in the cluster. For both the
   candidate and non-candidate hosts, the actual pending reason
   on each host is shown. For each pending reason, the number of
   hosts that show that reason is given. The actual reason
   messages appear from most to least common.

If no level is specified, the default is shown, as set by the
LSB\_BJOBS\_PENDREASON\_LEVEL parameter in the lsf.conf file.

If global limits are enabled (that is, the **GLOBAL\_LIMITS**
parameter is enabled in the lsb.params file and global limits are
defined in the lsb.globalpolicies file), the pending reason also
includes the total global limit amount for the resource
allocation limit in the Global field if the job is blocked
by the global limit value and the existing Limit Value
field is a dynamically changing number that is taken from the
global limits. In addition, the format of the job group name
changes to "/_job\_group_/@_cluster_" to show the
cluster name.

With IBM Spectrum LSF License Scheduler (LSF License Scheduler),
the pending reason also includes the project name for project
mode and the cluster name for cluster mode features. Jobs with an
invalid project name show the project name as a hyphen (-).
If a default project is configured for that feature, it shows
default as the project name.

If -l is also specified, the pending reason shows the names of
the hosts and any hosts that are on the job's host exclusion
list.

With the LSF multicluster capability, -l shows the names of hosts
in the local cluster.

Each pending reason is associated with one or more hosts and it
states the cause why these hosts are not allocated to run the
job. In situations where the job requests specific hosts (using
bsub -m), users may see reasons for unrelated hosts also being
displayed, together with the reasons associated with the
requested hosts.

In the case of host-based pre-execution failure, pending reasons
will be displayed.

The life cycle of a pending reason ends after the time indicated
by **PEND\_REASON\_UPDATE\_INTERVAL** in lsb.params.

When the job slot limit is reached for a job array (bsub -J
"jobArray[indexList]%job_slot_limit") the following message is
displayed:

The job array has reached its job slot limit.

<a name="examples"></a>

# Examples



bjobs -pl

Displays detailed information about all pending jobs of the
invoker. Also displays the names of any hosts that are in the
job's host exclusion list (that is, hosts that are excluded from
the job).

bjobs -ps

Display only pending and suspended jobs.

Parent topic: Options




**-pe**



Displays pending jobs that are eligible for scheduling.




# Categories



state

# Synopsis



**bjobs -pe**

# Description



A job that is in an eligible pending state is a job that LSF
would normally select for resource allocation, but is currently
pending because its priority is lower than other jobs. It is a
job that is eligible for scheduling and will be run if there are
sufficient resources to run it. In addition, for chunk jobs in
WAIT status, the time spent in the WAIT status is
counted as eligible pending time.

This option is valid only when **TRACK\_ELIGIBLE\_PENDINFO** in
lsb.params is set to Y or y (enabled).

Parent topic: Options




**-pei**


Displays pending jobs divided into lists of jobs that are
eligible for scheduling and ineligible for scheduling.


# Categories



state



# Synopsis



**bjobs -pei**


# Description



A job that is in an eligible pending state is a job that LSF
would normally select for resource allocation, but is currently
pending because its priority is lower than other jobs. It is a
job that is eligible for scheduling and will be run if there are
sufficient resources to run it.

An ineligible pending job remains pending even if there are
enough resources to run it and is therefore ineligible for
scheduling. Reasons for a job to remain pending, and therefore be
in an ineligible pending state, include the following:

*  The job has a start time constraint (specified with the -b
   option)

*  The job is suspended while pending (in a PSUSP state).

*  The queue of the job is made inactive by the administrator or
   by its time window.

*  The job's dependency conditions are not satisfied.

*  The job cannot fit into the run time window (**RUN\_WINDOW**)

*  Delayed scheduling is enabled for the job
   (**NEW\_JOB\_SCHED\_DELAY** is greater than zero)

*  The job's queue or application profile does not exist.

A job that is not under any of the ineligible pending state
conditions is treated as an eligible pending job. In addition,
for chunk jobs in WAIT status, the time spent in the
WAIT status is counted as eligible pending time.

This option is valid only when **TRACK\_ELIGIBLE\_PENDINFO** in
lsb.params is set to Y or y (enabled).

Parent topic: Options



**-pi**


Displays pending jobs that are ineligible for scheduling.



# Categories



state

# Synopsis



**bjobs -pi**

# Description



An ineligible pending job remains pending even if there are
enough resources to run it and is therefore ineligible for
scheduling. Reasons for a job to remain pending, and therefore be
in an ineligible pending state, include the following:

*  The job has a start time constraint (specified with the -b
   option)

*  The job is suspended while pending (in a PSUSP state).

*  The queue of the job is made inactive by the administrator or
   by its time window.

*  The job's dependency conditions are not satisfied.

*  The job cannot fit into the run time window (**RUN\_WINDOW**)

*  Delayed scheduling is enabled for the job
   (**NEW\_JOB\_SCHED\_DELAY** is greater than zero)

*  The job's queue or application profile does not exist.

A job that is not under any of the ineligible pending state
conditions is treated as an eligible pending job.

This option is valid only when **TRACK\_ELIGIBLE\_PENDINFO** in
lsb.params is set to Y or y (enabled).

Parent topic: Options




**-plan**



Filter for the PEND jobs that have an allocation plan. See
**ALLOCATION\_PLANNER** (lsb.params).




# Categories



filter


# Synopsis



**bjobs -plan**


# Description



To see the allocation plan, use the bjobs -plan option together
with the -l option.

Parent topic: Options



**-prio**



Displays the detailed absolute priority scheduling (APS)
factor values for all pending jobs.



# Categories



filter


# Synopsis



**bjobs -prio**


# Description



Displays the detailed priority factors and values for APS.
Run bqueues -r for further details on fair share user priority.

Parent topic: Options



.TH bjobs 1 "June 2023" "" ""
.ll 72

.ce 1000
**-psum**
.ce 0


Displays a summarized version of reasons for pending jobs.



# Categories



filter, format, state


# Synopsis



**bjobs -psum**

# Description



Displays the summarized number of jobs, hosts, and occurrences
for each pending reason.

**Note: **By default bjobs -psum is equivalent to bjobs -p
-psum.

By default, bjobs commands only access information about the
submitting user's own jobs. Therefore, bjobs -psum presents a
summary of only the submitting user's own pending jobs. If the -u
option is specified for a specific user, user group, or all users
(using the all keyword), pending jobs of the specific users are
summarized. The **SECURE\_JOB\_INFO\_LEVEL** parameter can define
different access control levels for all users. The -psum option
will only summarize the pending jobs that can be seen by the
users specified with **SECURE\_JOB\_INFO\_LEVEL**.


# Conflicting Options



Use only with the filter options that can return a list of
pending jobs, including the following: -app, -fwd, -G, -g, -J,
-Jd, -Lp, -m, -P, -p, -p(0~3), -pe, -pei, -pi, -q, -sla, -u


# Examples



bjobs -psum

Lists the top eligible and ineligible pending reasons in
descending order by the number of jobs. If a host reason exists,
further detailed host reasons are displayed in descending order
by occurrences. Occurrence is a per-job per-host based number,
counting the total times each job hits the reason on every single
host.

#bjobs -p -psum  
Pending reason summary: published Wed Mar 24 15:11:50 2016  
Summarizing 100 pending jobs in cluster (cluster1):  
    Individual host based reasons:                                   70 jobs  
    Job's requirements for reserving resource (lic1) not satisfied:  20 jobs  
    New job is waiting for scheduling:                               10 jobs   
  
Individual host based reasons  
    Load information unavailable:                                    160 occurrences  
    Closed by LSF administrator:                                      80 occurrences  
    Not specified in job submission:                                  40 occurrences  
    Job requirements for reserving resource (mem) not satisfied:      40 occurrences  


bjobs -q short -u user1 -psum

Displays summary of all pending job list submitted by user1 to
the queue short.

bjobs -p1 -psum

Displays summary of single key reasons (rather than all host
pending reason).

bjobs -p2 -psum

Displays summary of candidate host pending reasons (rather than
non-candidate host pending reason). If there are no pending jobs
with candidate host pending reasons, LSF will mention that there
are no pending jobs in the summary, such as Summarizing 0
pending jobs in cluster (cluster1).

bjobs -p3 -psum

Displays summary of both candidate host pending reasons and
non-candidate host pending reason.

Parent topic: Options




**-q**



Displays jobs in the specified queue.



<a name="categories"></a>

# Categories



filter


# Synopsis



**bjobs -q** queue_name



# Description



The command bqueues returns a list of queues configured in the
system, and information about the configurations of these queues.

In LSF multicluster capability, you cannot specify remote queues.


# Examples



bjobs -d -q short -m hostA -u user1

Displays all the recently finished jobs submitted by user1
to the queue short, and executed on the host hostA.

Parent topic: Options




**-r**



Displays running jobs.



# Categories



state

# Synopsis



**bjobs -r**

Parent topic: Options



**-rusage**



Displays jobs requesting the resources specified by the filter.



# Categories



filter

# Synopsis



**bjobs -rusage "**resource\_name[**,**resource\_name**,**. .
.]**"**

# Conflicting Options



Can be used with other state or filter options (for example, -p
or -r). Cannot be used with the -A option.

# Description



Filters the results and shows only jobs that request the
resource(s) specified.

If multiple resources are specified in the filter (separated by a
comma with no spaces), the "AND" relationship applies.

If a job is submitted with multiple alternative resources in
different sections (divided with "||"), each section will be
considered separately for rusage. For example:

bsub -R "{rusage[resource1=1]} || {rusage[resource2=2]}"

Or

bsub -R "{rusage[resource1=1 || resource2=2]}"

In this case, bjobs -rusage "resource1" or bjobs -rusage
"resource2" will return the job, but bjobs -rusage
"resource1,resource2" will not return the job.

# Examples



bjobs -rusage "resource1" Displays all jobs that are
requesting resource1.

bjobs -rusage "resource1" -p Displays all pending jobs that
are requesting resource1.

bjobs -r -q normal -rusage "resource1,resourceA" Displays
all running jobs in the queue "normal" that are requesting
resource1 and resourceA.

Parent topic: Options




**-s**



Displays suspended jobs, together with the suspending reason that
caused each job to become suspended.



# Categories



state

# Synopsis



**bjobs -s**


# Description



The suspending reason may not remain the same while the job stays
suspended. For example, a job may have been suspended due to the
paging rate, but after the paging rate dropped another load index
could prevent the job from being resumed. The suspending reason
is updated according to the load index. The reasons could be as
old as the time interval specified by **SBD\_SLEEP\_TIME** in
lsb.params. The reasons shown may not reflect the current load
situation.


# Examples



bjobs -ps

Display only pending and suspended jobs.

Parent topic: Options




**-script**



Displays the job script for the specified job from the LSF info
directory.



# Categories



filter

# Synopsis



**bjobs -script**

# Conflicting Options



Do not use with any other bjobs command options.

# Description



You must specify a single job ID or job array element ID when
using the -script command option. Multiple job IDs are not
supported.

Parent topic: Options



**-sla**



Displays jobs belonging to the specified service class.



# Categories



filter

# Synopsis



**bjobs -sla** service_class_name


# Description



bjobs also displays information about jobs assigned to a default
SLA configured with ENABLE_DEFAULT_EGO_SLA in lsb.params.

Use -sla with -g to display job groups attached to a time-based
service class. Once a job group is attached to a service class,
all jobs submitted to that group are subject to the SLA.

Use bsla to display the configuration properties of service
classes configured in lsb.serviceclasses, the default SLA
configured in lsb.params, and dynamic information about the state
of each service class.

<a name="examples"></a>

# Examples



bjobs -sla Sooke

Displays all jobs belonging to the service class Sooke.

Parent topic: Options




**-ss**


Displays summary information for LSF Session Scheduler tasks.



# Categories



filter


# Synopsis



**bjobs -ss**


# Conflicting Options



Do not use with the following options: -A, -aps, -fwd, -N, -W,
-WL, -WF, -WP.


# Description



Displays summary information for LSF Session Scheduler tasks
including the job ID, the owner, the job name (useful for job
arrays), the total number of tasks, the state of pending, done,
running, and exited session scheduler tasks.

-ss can only display the summary information for LSF Session
Scheduler tasks when the job session has started . -ss cannot
display the information while LSF Session Scheduler job is still
pending.

The frequency of the updates of this information is based on the
parameters **SSCHED\_UPDATE\_SUMMARY\_INTERVAL** and
**SSCHED\_UPDATE\_SUMMARY\_BY\_TASK**.

Parent topic: Options




**-sum**



Displays summary information about unfinished jobs.




# Categories



state, format

# Synopsis



**bjobs -sum**

# Description



bjobs -sum displays the count of job slots in the following
states: running (RUN), system suspended (SSUSP), user suspended
(USUSP), suspended while pending (PSUSP), pending (PEND),
forwarded to remote clusters and pending (FWD_PEND), and UNKNOWN.

bjobs -sum displays the job slot count only for the user’s own
jobs.

Use -sum with other options (like -m, -P, -q, and -u) to filter
the results. For example, bjobs -sum -u user1 displays job slot
counts just for user user1.


# Examples



% bjobs -sum  
RUN        SSUSP       USUSP      UNKNOWN    PEND       FWD_PEND  PSUSP      
123        456         789        5          5          3         3          


To filter the -sum results to display job slot counts just for
user user1, run bjobs -sum -u user1:

% bjobs -sum -u user1  
RUN        SSUSP       USUSP      UNKNOWN    PEND       FWD_PEND  PSUSP      
20         10          10         0           5          0        2          


Parent topic: Options



**-U**



Displays jobs that are associated with the specified advance
reservation.




# Categories



filter


# Synopsis



**bjobs -U** adv_reservation_id


# Description



To view all jobs that are associated with the specified advance
reservation, run the -U option with 0 as the job ID.

To see if a single job is associated with an advance reservation,
run the -U option with one specified job ID. The output displays
the job information only if the job is associated with the
specified advance reservation. Otherwise, the output displays an
error message.

To see which jobs in a specified list are associated with an
advance reservation, run the -U and -l options together with
multiple job IDs. The output displays detailed job information
only if the jobs are associated with the specified advanced
reservation. Otherwise, the output displays an error message for
each job that is not associated with the advanced reservation.

**Note: **To view a list of job IDs, you must use the -U and -l
options together. If you use the -U option without -l for
multiple job IDs, the output displays all jobs regardless of
whether they are associated with the advance reservation unless
the job IDs are not valid.

# Examples



Displays all jobs that are associated with the reservation ID
user1#2:

bjobs -U user1#2 0

Displays one job with ID 101 if it is associated with the
reservation ID user1#2:

bjobs -U user1#2 101

Displays any jobs from a specified list (with IDs 101, 102, 103,
and 104) that are associated with the reservation ID
user1#2:

bjobs -U user1#2 -l 101 102 103 104

Parent topic: Options




**-u**



Displays jobs that were submitted by the specified users or user
groups.


# Categories



filter


# Synopsis



**bjobs -u** user_name ... | **-u** user_group ... | -u all

# Conflicting Options



Do not use with the -G option.

# Description



The keyword all specifies all users. To specify a Windows user
account, include the domain name in uppercase letters and use a
single backslash (_DOMAIN\_NAME_\_user\_name_) in a Windows
command line or a double backslash
(_DOMAIN\_NAME_\\_user\_name_) in a UNIX command line.

<a name="examples"></a>

# Examples



bjobs -u all -a

Displays all jobs of all users.

bjobs -d -q short -m hostA -u user1

Displays all the recently finished jobs submitted by user1
to the queue short, and executed on the host hostA.

Parent topic: Options




**-UF**


Displays unformatted job detail information.




# Categories



format

# Synopsis



**bjobs -UF**

# Description



This makes it easy to write scripts for parsing keywords on
bjobs. The results of this option have no wide control for the
output. Each line starts from the beginning of the line.
Information for **SCHEDULING PARAMETERS** and PENDING
REASONS remain formatted. The resource usage message lines
ending without any separator have a semicolon added to separate
their different parts. The first line and all lines starting with
the time stamp are displayed unformatted in a single line. There
is no line length and format control.

# Examples



% bjobs -UF  
Job &lt;1&gt;, User &lt;lsfuser&gt;, Project &lt;default&gt;, Status &lt;RUN&gt;, Queue &lt;normal&gt;, Command &lt;./pi_css5 10000000&gt;, Share group charged &lt;/lsfuser&gt;  
Tue May  6 15:45:10: Submitted from host &lt;hostA&gt;, CWD &lt;/home/lsfuser&gt;;  
Tue May  6 15:45:11: Started on &lt;hostB&gt;, Execution Home &lt;/home/lsfuser&gt;, Execution CWD &lt;/home/lsfuser&gt;;  
  
 SCHEDULING PARAMETERS:  
           r15s   r1m  r15m   ut      pg    io   ls    it    tmp    swp    mem  
 loadSched   -     -     -     -       -     -    -     -     -      -      -  
 loadStop    -     -     -     -       -     -    -     -     -      -      -  
  
 RESOURCE REQUIREMENT DETAILS:  
 Combined: select[type == local] order[r15s:pg]  
 Effective: select[type == local] order[r15s:pg]  


Parent topic: Options




**-W**


Provides resource usage information for: PROJ\_NAME,
CPU\_USED, MEM, SWAP, PIDS,
START\_TIME, FINISH\_TIME.



# Categories



format

# Synopsis



**bjobs -W**


# Description



Displays resource information for jobs that belong to you only if
you are not logged in as an administrator.

Parent topic: Options




**-w**


Wide format. Displays job information without truncating fields.


# Categories



format

# Synopsis



**bjobs -w**

Parent topic: Options




**-WF**


Displays an estimated finish time for running or pending jobs.
For done or exited jobs, displays the actual finish time.



# Categories



format

# Synopsis



**bjobs -WF**

# Output



The output for the -WF, -WL, and -WP options are in the following
format:

_hours_:_minutes_ _status_

where _status_ is one of the following:

*  X: The real run time has exceeded the estimated run time
   configured in the application profile (**RUNTIME** parameter
   in lsb.applications) or at the job level (bsub -We option).

*  L: A run limit exists but the job does not have an
   estimated run time.

*  E: An estimated run time exists and has not been
   exceeded.

Parent topic: Options




**-WL**



Displays the estimated remaining run time of jobs.


# Categories



format

# Synopsis



**bjobs -WL**
# Output



The output for the -WF, -WL, and -WP options are in the following
format:

_hours_:_minutes_ _status_

where _status_ is one of the following:

*  X: The real run time has exceeded the estimated run time
   configured in the application profile (**RUNTIME** parameter
   in lsb.applications) or at the job level (bsub -We option).

*  L: A run limit exists but the job does not have an
   estimated run time.

*  E: An estimated run time exists and has not been
   exceeded.

Parent topic: Options




**-WP**



Displays the current estimated completion percentage of jobs.




# Categories



format

# Synopsis



**bjobs -WP**

# Output



The output for the -WF, -WL, and -WP options are in the following
format:

_hours_:_minutes_ _status_

where _status_ is one of the following:

*  X: The real run time has exceeded the estimated run time
   configured in the application profile (**RUNTIME** parameter
   in lsb.applications) or at the job level (bsub -We option).

*  L: A run limit exists but the job does not have an
   estimated run time.

*  E: An estimated run time exists and has not been
   exceeded.

Parent topic: Options




**-X**



Displays noncondensed output for host groups and compute units.




# Categories



format

# Synopsis



**bjobs -X**


# Examples



bjobs -X 101 102 203 509

Display jobs with job ID 101, 102, 203, and 509 as noncondensed
output even if these jobs belong to hosts in condensed groups.

Parent topic: Options




**-x**



Displays unfinished jobs that have triggered a job exception
(overrun, underrun, idle,
runtime\_est\_exceeded).




# Categories



state

# Synopsis



**bjobs -x**

# Description



Use with the -l option to show the actual exception status. Use
with -a to display all jobs that have triggered a job exception.

Parent topic: Options




**job\_id**



Specifies the jobs or job arrays that bjobs displays.



# Synopsis



**bjobs** [options] [job_id |
**"**job\_id**[**index\_list**]"** ... ]


# Description



If you use -A, specify job array IDs without the index list.

In LSF multicluster capability job forwarding mode, you can use
the local job ID and cluster name to retrieve the job details
from the remote cluster. The query syntax is:

bjobs _submission\_job\_id_@_submission\_cluster\_name_

For job arrays, the query syntax is:

bjobs
"_submission\_job\_id_[_index_]"@_submission\_cluster\_name_

The advantage of using submission_job_id@submission_cluster_name
instead of bjobs -l job_id is that you can use
submission_job_id@submission_cluster_name as an alias to query a
local job in the execution cluster without knowing the local job
ID in the execution cluster. The bjobs output is identical no
matter which job ID you use (local job ID or
_submission\_job\_id_@_submission\_cluster\_name_).

You can use bjobs 0 to find all jobs in your local cluster, but
bjobs 0@submission_cluster_name is not supported.

<a name="examples"></a>

# Examples



bjobs 101 102 203 509

Display jobs with job_ID 101, 102, 203, and 509.

bjobs -X 101 102 203 509

Display jobs with job ID 101, 102, 203, and 509 as uncondensed
output even if these jobs belong to hosts in condensed groups.

Parent topic: Options




**-h**


Displays a description of the specified category, command option,
or sub-option to stderr and exits.



# Synopsis



**bjobs -h**[**elp**] [category ...] [option ...]


# Description



You can abbreviate the -help option to -h.

Run bjobs -h (or bjobs -help) without a command option or
category name to display the bjobs command description.


# Examples



bjobs -h filter

Displays a description of the filter category and the bjobs
command options belonging to this category.

bjobs -h -o

Displays a detailed description of the bjobs -o option.

Parent topic: Options




**-V**


Prints LSF release version to stderr and exits.



# Synopsis



**bjobs -V**


# Conflicting Options



Do not use with any other option except -h (bjobs -h -V).

Parent topic: Options




**Description**



By default, displays information about your own pending, running,
and suspended jobs.

bjobs displays output for condensed host groups and compute
units. These host groups and compute units are defined by
CONDENSE in the HostGroup or ComputeUnit section of
lsb.hosts. These groups are displayed as a single entry with the
name as defined by GROUP_NAME or NAME in lsb.hosts. The -l and -X
options display noncondensed output.

If you defined the parameter **LSB\_SHORT\_HOSTLIST=1** in the
lsf.conf file, parallel jobs running in the same condensed host
group or compute unit are displayed as an abbreviated list.

For resizable jobs, bjobs displays the auto-resizable attribute
and the resize notification command.

To display older historical information, use bhist.

<a name="output-default-display"></a>

# Output: Default Display



Pending jobs are displayed in the order in which they are
considered for dispatch. Jobs in higher priority queues are
displayed before those in lower priority queues. Pending jobs in
the same priority queues are displayed in the order in which they
were submitted but this order can be changed by using the
commands btop or bbot. If more than one job is dispatched to a
host, the jobs on that host are listed in the order in which they
are considered for scheduling on this host by their queue
priorities and dispatch times. Finished jobs are displayed in the
order in which they were completed.

A listing of jobs is displayed with the following fields:

**JOBID **  
         The job ID that LSF assigned to the job.

**USER**  
         The user who submitted the job.

**STAT**  
         The current status of the job (see JOB STATUS below).

**QUEUE**  
         The name of the job queue to which the job belongs. If
         the queue to which the job belongs has been removed from
         the configuration, the queue name is displayed as
         lost\_and\_found. Use bhist to get the original
         queue name. Jobs in the lost\_and\_found queue
         remain pending until they are switched with the bswitch
         command into another queue.

         In a LSF multicluster capability resource leasing
         environment, jobs scheduled by the consumer cluster
         display the remote queue name in the format
         _queue\_name_@_cluster\_name_. By default, this
         field truncates at 10 characters, so you might not see
         the cluster name unless you use -w or -l.

**FROM\_HOST**  
         The name of the host from which the job was submitted.

         With the LSF multicluster capability, if the host is in
         a remote cluster, the cluster name and remote job ID are
         appended to the host name, in the format
         _host\_name_@_cluster\_name_:_job\_ID_. By
         default, this field truncates at 11 characters; you
         might not see the cluster name and job ID unless you use
         -w or -l.

**EXEC\_HOST**  
         The name of one or more hosts on which the job is
         executing (this field is empty if the job has not been
         dispatched). If the host on which the job is running has
         been removed from the configuration, the host name is
         displayed as lost\_and\_found. Use bhist to get the
         original host name.

         If the host is part of a condensed host group or compute
         unit, the host name is displayed as the name of the
         condensed group.

         If you configure a host to belong to more than one
         condensed host groups using wildcards, bjobs can display
         any of the host groups as execution host name.

**JOB_NAME **  
         The job name assigned by the user, or the command string
         assigned by default at job submission with bsub. If the
         job name is too long to fit in this field, then only the
         latter part of the job name is displayed.

         The displayed job name or job command can contain up to
         4094 characters for UNIX, or up to 255 characters for
         Windows.

**SUBMIT_TIME **  
         The submission time of the job.

<a name="output-long-format-l"></a>

# Output: Long Format (-L)



The -l option displays a long format listing with the following
additional fields:

**Job**  
         The job ID that LSF assigned to the job.

**User**  
         The ID of the user who submitted the job.

**Project**  
         The project the job was submitted from.

**Application Profile**  
         The application profile the job was submitted to.

**Command **  
         The job command.

**CWD **  
         The current working directory on the submission host.

**Data requirement requested **  
         Indicates that the job has data requirements.

**Execution CWD **  
         The actual CWD used when job runs.

**Host file**  
         The path to a user-specified host file used when
         submitting or modifying a job.

**Initial checkpoint period**  
         The initial checkpoint period specified at the job
         level, by bsub -k, or in an application profile with
         CHKPNT_INITPERIOD.

**Checkpoint period**  
         The checkpoint period specified at the job level, by
         bsub -k, in the queue with CHKPNT, or in an application
         profile with CHKPNT_PERIOD.

**Checkpoint directory**  
         The checkpoint directory specified at the job level, by
         bsub -k, in the queue with CHKPNT, or in an application
         profile with CHKPNT_DIR.

**Migration threshold**  
         The migration threshold specified at the job level, by
         bsub -mig.

**Post-execute Command **  
         The post-execution command specified at the job-level,
         by bsub -Ep.

**PENDING REASONS **  
         The reason the job is in the PEND or PSUSP state. The
         names of the hosts associated with each reason are
         displayed when both -p and -l options are
         specified.

**SUSPENDING REASONS **  
         The reason the job is in the USUSP or SSUSP state.

         **loadSched **  
                  The load scheduling thresholds for the job.

         **loadStop **  
                  The load suspending thresholds for the job.

**JOB STATUS**  
         Possible values for the status of a job include:

         **PEND **  
                  The job is pending. That is, it has not yet
                  been started.

         **PROV**  
                  The job has been dispatched to a power-saved
                  host that is waking up. Before the job can be
                  sent to the sbatchd, it is in a PROV state.

         **PSUSP**  
                  The job has been suspended, either by its owner
                  or the LSF administrator, while pending.

         **RUN **  
                  The job is currently running.

         **USUSP **  
                  The job has been suspended, either by its owner
                  or the LSF administrator, while running.

         **SSUSP**  
                  The job has been suspended by LSF. The
                  following are examples of why LSF suspended the
                  job:

                  *  The load conditions on the execution host or
                     hosts have exceeded a threshold according to
                     the loadStop vector defined for the
                     host or queue.

                  *  The run window of the job's queue is closed.
                     See bqueues(1), bhosts(1), and
                     lsb.queues(5).

         **DONE**  
                  The job has terminated with status of 0.

         **EXIT**  
                  The job has terminated with a non-zero status –
                  it may have been aborted due to an error in its
                  execution, or killed by its owner or the LSF
                  administrator.

                  For example, exit code 131 means that the job
                  exceeded a configured resource usage limit and
                  LSF killed the job.

         **UNKWN**  
                  mbatchd has lost contact with the sbatchd on
                  the host on which the job runs.

         **WAIT**  
                  For jobs submitted to a chunk job queue,
                  members of a chunk job that are waiting to run.

         **ZOMBI**  
                  A job becomes ZOMBI if:

                  *  A non-rerunnable job is killed by bkill
                     while the sbatchd on the execution host is
                     unreachable and the job is shown as UNKWN.

                  *  After the execution host becomes available,
                     LSF tries to kill the ZOMBI job. Upon
                     successful termination of the ZOMBI job, the
                     job's status is changed to EXIT.

                     With the LSF multicluster capability, when a
                     job running on a remote execution cluster
                     becomes a ZOMBI job, the execution cluster
                     treats the job the same way as local ZOMBI
                     jobs. In addition, it notifies the
                     submission cluster that the job is in ZOMBI
                     state and the submission cluster requeues
                     the job.

**RUNTIME**  
         Estimated run time for the job, specified by bsub -We or
         bmod -We, -We+, -Wep.

         The following information is displayed when running
         bjobs -WL, -WF, or -WP.

         **TIME\_LEFT**  
                  The estimated run time that the job has
                  remaining. Along with the time if applicable,
                  one of the following symbols may also display.

                  *  E: The job has an estimated run time that
                     has not been exceeded.

                  *  L: The job has a hard run time limit
                     specified but either has no estimated run
                     time or the estimated run time is more than
                     the hard run time limit.

                  *  X: The job has exceeded its estimated run
                     time and the time displayed is the time
                     remaining until the job reaches its hard run
                     time limit.

                  *  A dash indicates that the job has no
                     estimated run time and no run limit, or that
                     it has exceeded its run time but does not
                     have a hard limit and therefore runs until
                     completion.

                  If there is less than a minute remaining, 0:0
                  displays.

         **FINISH\_TIME**  
                  The estimated finish time of the job. For
                  done/exited jobs, this is the actual finish
                  time. For running jobs, the finish time is the
                  start time plus the estimated run time (where
                  set and not exceeded) or the start time plus
                  the hard run limit.

                  *  E: The job has an estimated run time that
                     has not been exceeded.

                  *  L: The job has a hard run time limit
                     specified but either has no estimated run
                     time or the estimated run time is more than
                     the hard run time limit.

                  *  X: The job has exceeded its estimated run
                     time and had no hard run time limit set. The
                     finish time displayed is the estimated run
                     time remaining plus the start time.

                  *  A dash indicates that the pending,
                     suspended, or job with no run limit has no
                     estimated finish time.

         **%COMPLETE**  
                  The estimated completion percentage of the job.

                  *  E: The job has an estimated run time that
                     has not been exceeded.

                  *  L: The job has a hard run time limit
                     specified but either has no estimated run
                     time or the estimated run time is more than
                     the hard run time limit.

                  *  X: The job has exceeded its estimated run
                     time and had no hard run time limit set.

                  *  A dash indicates that the jobs is pending,
                     or that it is running or suspended, but has
                     no run time limit specified.

         **Note: **For jobs in the state UNKNOWN, the job run
         time estimate is based on internal counting by the job's
         mbatchd.

**RESOURCE USAGE **  
         For the LSF multicluster capability job forwarding
         model, this information is not shown if the LSF
         multicluster capability resource usage updating is
         disabled. Use **LSF\_HPC\_EXTENSIONS="HOST\_RUSAGE"** in
         lsf.conf to specify host-based resource usage.

         The values for the current usage of a job include:

         **HOST**  
                  For host-based resource usage, specifies the
                  host.

         **CPU time**  
                  Cumulative total CPU time in seconds of all
                  processes in a job. For host-based resource
                  usage, the cumulative total CPU time in seconds
                  of all processes in a job running on a host.

         **IDLE\_FACTOR**  
                  Job idle information (CPU time/runtime) if
                  JOB_IDLE is configured in the queue, and the
                  job has triggered an idle exception.

         **MEM**  
                  Total resident memory usage of all processes in
                  a job. For host-based resource usage, the total
                  resident memory usage of all processes in a job
                  running on a host. The sum of host-based
                  rusage may not equal the total job
                  rusage, since total job rusage is
                  the maximum historical value.

                  Memory usage unit is scaled automatically based
                  on the value. Use LSF_UNIT_FOR_LIMITS in
                  lsf.conf to specify the smallest unit for
                  display (KB, MB, GB, TB, PB, or EB).

         **SWAP**  
                  Total virtual memory and swap usage of all
                  processes in a job. For host-based resource
                  usage, the total virtual memory usage of all
                  processes in a job running on a host. The sum
                  of host-based usage may not equal the total job
                  usage, since total job usage is the maximum
                  historical value.

                  Swap usage unit is scaled automatically based
                  on the value. Use the **LSF\_UNIT\_FOR\_LIMITS**
                  in the lsf.conf file to specify the smallest
                  unit for display (KB, MB, GB, TB, PB, or EB).

                  By default, LSF collects both memory and swap
                  usage through PIM:

                  *  If the **EGO\_PIM\_SWAP\_REPORT=n** parameter
                     is set in the lsf.conf file (this is the
                     default), swap usage is virtual memory (VSZ)
                     of the entire job process.

                  *  If the **EGO\_PIM\_SWAP\_REPORT=y** parameter
                     is set in the lsf.conf file, the resident
                     set size (RSS) is subtracted from the
                     virtual memory usage. RSS is the portion of
                     memory occupied by a process that is held in
                     main memory. Swap usage is collected as the
                     _VSZ_ - _RSS_.



                  If memory enforcement through the Linux cgroup
                  memory subsystem is enabled with the
                  **LSF\_LINUX\_CGROUP\_ACCT=y** parameter in the
                  lsf.conf file, LSF uses the cgroup memory
                  subsystem to collect memory and swap usage of
                  all processes in a job.

         **NTHREAD**  
                  Number of currently active threads of a job.

         **PGID**  
                  Currently active process group ID in a job. For
                  host-based resource usage, the currently active
                  process group ID in a job running on a host.

         **PIDs**  
                  Currently active processes in a job. For
                  host-based resource usage, the currently active
                  processes in a job running on a host.

**RESOURCE LIMITS**  
         The hard resource usage limits that are imposed on the
         jobs in the queue (see getrlimit(2) and lsb.queues(5)).
         These limits are imposed on a per-job and a per-process
         basis.

         The possible per-job resource usage limits are:

         *  CPULIMIT

         *  TASKLIMIT

         *  MEMLIMIT

         *  SWAPLIMIT

         *  PROCESSLIMIT

         *  THREADLIMIT

         *  OPENFILELIMIT

         *  HOSTLIMIT_PER_JOB

         The possible UNIX per-process resource usage limits are:

         *  RUNLIMIT

         *  FILELIMIT

         *  DATALIMIT

         *  STACKLIMIT

         *  CORELIMIT

         If a job submitted to the queue has any of these limits
         specified (see bsub(1)), then the lower of the
         corresponding job limits and queue limits are used for
         the job.

         If no resource limit is specified, the resource is
         assumed to be unlimited. User shell limits that are
         unlimited are not displayed.

**EXCEPTION STATUS**  
         Possible values for the exception status of a job
         include:

         **idle**  
                  The job is consuming less CPU time than
                  expected. The job idle factor (CPU
                  time/runtime) is less than the configured
                  JOB_IDLE threshold for the queue and a job
                  exception has been triggered.

         **overrun**  
                  The job is running longer than the number of
                  minutes specified by the JOB_OVERRUN threshold
                  for the queue and a job exception has been
                  triggered.

         **underrun**  
                  The job finished sooner than the number of
                  minutes specified by the JOB_UNDERRUN threshold
                  for the queue and a job exception has been
                  triggered.

**Requested resources**  
         Shows all the resource requirement strings you specified
         in the bsub command.

**Execution **rusage  
         This is shown if the combined RES_REQ has an
         rusage or || construct. The chosen
         alternative will be denoted here.

**Synchronous Execution**  
         Job was submitted with the -K option. LSF submits the
         job and waits for the job to complete.

**JOB_DESCRIPTION **  
         The job description assigned by the user. This field is
         omitted if no job description has been assigned.

         The displayed job description can contain up to 4094
         characters.

**MEMORY USAGE**  
         Displays peak memory usage and average memory usage. For
         example:

         MEMORY USAGE:  
         MAX MEM:11 Mbytes; AVG MEM:6 Mbytes

         Starting in Fix Pack 14, displays peak memory usage,
         average memory usage, and memory usage efficiency. For
         example:

         MEMORY USAGE:  
         MAX MEM:11 Mbytes; AVG MEM:6 Mbytes; MEM Efficiency: 10.00%

         where, MEM Efficiency is calculated using the
         following formula:

         MEM Efficiency = (MAX MEM / MEM requested in bsub -R "rusage[mem=]") * 100%

         If no memory in requested in the bsub command, the
         MEM Efficiency value will be 0.

         You can adjust the rusage value accordingly, the
         next time for the same job submission, if consumed
         memory is larger or smaller than current rusage
         amount.

**CPU USAGE**  
         Available starting in Fix Pack 14: displays the maximum
         number of CPUs used while running the job (CPU peak),
         duration for CPU to peak (in seconds), CPU average
         efficiency, and CPU peak efficiency. For example:

         CPU USAGE:  
          CPU PEAK: 4.24; CPU PEAK DURATION: 54 second(s)  
          CPU AVERAGEG EFFICIENCY: 99.55%; CPU PEAK EFFICIENCY: 106.02%

         *  CPU PEAK is the maximum number of CPUs used for
            running the job.

         *  CPU PEAK DURATION is the duration, in seconds,
            to reach the CPU peak for the job.

         *  CPU AVERAGE EFFICIENCY is calculated using the
            following formula:

            CPU AVERAGE EFFICIENCY = (CPU_TIME / (JOB_RUN_TIME * CPU_REQUESTED)) * 100%

         *  CPU PEAK EFFICIENCY is calculated using the
            following formula:

            CPU PEAK Efficiency = (CPU PEAK / CPU_REQUESTED) * 100%

**RESOURCE REQUIREMENT DETAILS**  
         Displays the configured level of resource requirement
         details. The **BJOBS\_RES\_REQ\_DISPLAY** parameter in
         lsb.params controls the level of detail that this column
         displays, which can be as follows:

         *  none - no resource requirements are displayed (this
            column is not displayed in the -l output).

         *  brief - displays the combined and effective resource
            requirements.

         *  full - displays the job, app, queue, combined and
            effective resource requirements.

**Requested Network**  
         Displays network resource information for IBM Parallel
         Edition (PE) jobs submitted with the bsub -network
         option. It does not display network resource information
         from the **NETWORK\_REQ** parameter in lsb.queues or
         lsb.applications.

         For example:

         bjobs -l  
         Job &lt;2106&gt;, User &lt;user1&gt;;, Project &lt;default&gt;;, Status &lt;RUN&gt;;, Queue &lt;normal&gt;,  
                              Command &lt;my_pe_job&gt;  
         Fri Jun  1 20:44:42: Submitted from host &lt;hostA&gt;, CWD &lt;$HOME&gt;, Requested Network  
                               &lt;protocol=mpi: mode=US: type=sn_all: instance=1: usage=dedicated&gt;

         If mode=IP is specified for the PE job, instance
         is not displayed.

**DATA REQUIREMENTS**  
         When you use -data, displays a list of requested files
         for jobs with data requirements.


# Output: Forwarded Job Information



The -fwd option filters output to display information on
forwarded jobs in the LSF multicluster capability job forwarding
mode. The following additional fields are displayed:

**CLUSTER **  
         The name of the cluster to which the job was forwarded.

**FORWARD_TIME **  
         The time that the job was forwarded.

# Output: Job Array Summary Information



Use -A to display summary information about job arrays. The
following fields are displayed:

**JOBID **  
         Job ID of the job array.

**ARRAY_SPEC **  
         Array specification in the format of
         _name_[_index_]. The array specification may be
         truncated, use -w option together with -A to show the
         full array specification.

**OWNER **  
         Owner of the job array.

**NJOBS **  
         Number of jobs in the job array.

**PEND **  
         Number of pending jobs of the job array.

**RUN**  
         Number of running jobs of the job array.

**DONE **  
         Number of successfully completed jobs of the job array.

**EXIT **  
         Number of unsuccessfully completed jobs of the job
         array.

**SSUSP **  
         Number of LSF system suspended jobs of the job array.

**USUSP **  
         Number of user suspended jobs of the job array.

**PSUSP **  
         Number of held jobs of the job array.

<a name="output-lsf-session-scheduler-job-summary-informationlsf-session-scheduler-job-summary-informationjob-summary-information"></a>

# Output: Lsf Session Scheduler Job Summary Informationlsf Session Scheduler Job Summary Informationjob Summary Information



**JOBID**  
         Job ID of the Session Scheduler job.

**OWNER**  
         Owner of the Session Scheduler job.

**JOB_NAME **  
         The job name assigned by the user, or the command string
         assigned by default at job submission with bsub. If the
         job name is too long to fit in this field, then only the
         latter part of the job name is displayed.

         The displayed job name or job command can contain up to
         4094 characters for UNIX, or up to 255 characters for
         Windows.

**NTASKS**  
         The total number of tasks for this Session Scheduler
         job.

**PEND**  
         Number of pending tasks of the Session Scheduler job.

**RUN**  
         Number of running tasks of the Session Scheduler job.

**DONE**  
         Number of successfully completed tasks of the Session
         Scheduler job.

**EXIT**  
         Number of unsuccessfully completed tasks of the Session
         Scheduler job.


# Output: Unfinished Job Summary Information



Use -sum to display summary information about unfinished jobs.
The count of job slots for the following job states is displayed:

**RUN **  
         The job is running.

**SSUSP**  
         The job has been suspended by LSF.

**USUSP **  
         The job has been suspended, either by its owner or the
         LSF administrator, while running.

**UNKNOWN**  
         mbatchd has lost contact with the sbatchd on the host
         where the job was running.

**PEND **  
         The job is pending, which may include PSUSP and
         chunk job WAIT. When -sum is used with -p in the
         LSF multicluster capability, WAIT jobs are not
         counted as PEND or FWD\_PEND. When -sum is
         used with -r, WAIT jobs are counted as PEND
         or FWD\_PEND.

**FWD_PEND **  
         The job is pending and forwarded to a remote cluster.
         The job has not yet started in the remote cluster.

# Output: Affinity Resource Requirements Information (-L -Aff)



Use -l -aff to display information about CPU and memory affinity
resource requirements for job tasks. A table with the heading
AFFINITY is displayed containing the detailed affinity
information for each task, one line for each allocated processor
unit. CPU binding and memory binding information are shown in
separate columns in the display.

**HOST**  
         The host the task is running on

**TYPE**  
         Requested processor unit type for CPU binding. One of
         numa, socket, core, or thread.

**LEVEL**  
         Requested processor unit binding level for CPU binding.
         One of numa, socket, core, or
         thread. If no CPU binding level is requested, a
         dash (-) is displayed.

**EXCL**  
         Requested processor unit binding level for exclusive CPU
         binding. One of numa, socket, or core.
         If no exclusive binding level is requested, a dash
         (-) is displayed.

**IDS**  
         List of physical or logical IDs of the CPU allocation
         for the task.

         The list consists of a set of paths, represented as a
         sequence integers separated by slash characters
         (/), through the topology tree of the host. Each
         path identifies a unique processing unit allocated to
         the task. For example, a string of the form
         3/0/5/12 represents an allocation to thread 12 in
         core 5 of socket 0 in NUMA node 3. A string of the form
         2/1/4represents an allocation to core 4 of socket
         1 in NUMA node 2. The integers correspond to the node ID
         numbers displayed in the topology tree from bhosts -aff.

**POL**  
         Requested memory binding policy. Eitherlocal or
         pref. If no memory binding is requested, a dash
         (-) is displayed.

**NUMA**  
         ID of the NUMA node that the task memory is bound to. If
         no memory binding is requested, a dash (-) is
         displayed.

**SIZE**  
         Amount of memory allocated for the task on the NUMA
         node.

For example the following job starts 6 tasks with the following
affinity resource requirements:

bsub -n 6 -R"span[hosts=1] rusage[mem=100]affinity[core(1,same=socket,  
exclusive=(socket,injob)):cpubind=socket:membind=localonly:distribute=pack]" myjob  
Job &lt;6&gt; is submitted to default queue &lt;normal&gt;.

bjobs -l -aff 6  
  
Job &lt;6&gt;, User &lt;user1&gt;, Project &lt;default&gt;, Status &lt;RUN&gt;, Queue &lt;normal&gt;, Comman  
                     d &lt;myjob1&gt;  
Thu Feb 14 14:13:46: Submitted from host &lt;hostA&gt;, CWD &lt;$HOME&gt;, 6 Task(s),   
                     Requested Resources &lt;span[hosts=1] rusage[mem=10  
                     0]affinity[core(1,same=socket,exclusive=(socket,injob)):cp  
                     ubind=socket:membind=localonly:distribute=pack]&gt;;  
Thu Feb 14 14:15:07: Started 6 Task(s) on Hosts &lt;hostA&gt; &lt;hostA&gt; &lt;hostA&gt; &lt;hostA&gt;  
                     &lt;hostA&gt; &lt;hostA&gt;, Allocated 6 Slot(s) on Hosts &lt;hostA&gt;  
                     &lt;hostA&gt; &lt;hostA&gt; &lt;hostA&gt; &lt;hostA&gt; &lt;hostA&gt;, Execution Home   
                     &lt;/home/user1&gt;, Execution CWD &lt;/home/user1&gt;;  
  
 SCHEDULING PARAMETERS:  
           r15s   r1m  r15m   ut      pg    io   ls    it    tmp    swp    mem  
 loadSched   -     -     -     -       -     -    -     -     -      -      -  
 loadStop    -     -     -     -       -     -    -     -     -      -      -  
  
 RESOURCE REQUIREMENT DETAILS:  
 Combined: select[type == local] order[r15s:pg] rusage[mem=100.00] span[hosts=1  
                     ] affinity[core(1,same=socket,exclusive=(socket,injob))*1:  
                     cpubind=socket:membind=localonly:distribute=pack]  
 Effective: select[type == local] order[r15s:pg] rusage[mem=100.00] span[hosts=  
                     1] affinity[core(1,same=socket,exclusive=(socket,injob))*1  
                     :cpubind=socket:membind=localonly:distribute=pack]  
  
 AFFINITY:  
                     CPU BINDING                          MEMORY BINDING  
                     ------------------------             --------------------  
 HOST                TYPE   LEVEL  EXCL   IDS             POL   NUMA SIZE  
 hostA               core   socket socket /0/0/0          local 0    16.7MB  
 hostA               core   socket socket /0/1/0          local 0    16.7MB  
 hostA               core   socket socket /0/2/0          local 0    16.7MB  
 hostA               core   socket socket /0/3/0          local 0    16.7MB  
 hostA               core   socket socket /0/4/0          local 0    16.7MB  
 hostA               core   socket socket /0/5/0          local 0    16.7MB  
  
  

# Output: Data Requirements Information (-L -Data)



Use -l -data to display detailed information about jobs with data
requirements. The heading DATA REQUIREMENTS is displayed
followed by a list of the files requested by the job.

For example:

bjobs -l -data 1962  
Job &lt;1962&gt;, User &lt;user1&gt;, Project &lt;default&gt;, Status &lt;PEND&gt;, Queue  
        &lt;normal&gt;,Command &lt;my_data_job&gt;  
Fri Sep 20 16:31:17: Submitted from host &lt;hb05b10&gt;, CWD   
        &lt;$HOME/source/user1/work&gt;, Data requirement requested;  
 PENDING REASONS:  
 Job is waiting for its data requirement to be satisfied;  
  
 SCHEDULING PARAMETERS:  
           r15s  r1m  r15m  ut  pg  io  ls  it  tmp  swp  mem  
 loadSched   -     -   -     -       -     -    -     -     -      -      -  
 loadStop    -     -   -     -       -     -    -     -     -      -      -  
  
 RESOURCE REQUIREMENT DETAILS:  
 Combined: select[type == local] order[r15s:pg]  
 Effective: -  
  
 DATA REQUIREMENTS:   
 FILE: hostA:/home/user1/data2  
 SIZE: 40 MB  
 MODIFIED: Thu Aug 14 17:01:57  
  
 FILE: hostA:/home/user1/data3  
 SIZE: 40 MB  
 MODIFIED: Fri Aug 15 16:32:45  
  
 FILE: hostA:/home/user1/data4  
 SIZE: 500 MB  
 MODIFIED: Mon Apr 14 17:15:56  


# See Also



bsub, bkill, bhosts, bmgroup, bclusters, bqueues, bhist, bresume,
bsla, bstop, lsb.params, lsb.serviceclasses, mbatchd

Parent topic: bjobs
