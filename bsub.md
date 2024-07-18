# bsub(1)






**bsub**



Submits a job to LSF by running the specified command and its
arguments.





# Synopsis



**bsub** [options] command [arguments]

**bsub** [options] job_script|

**bsub -R "**res\_req**"** [**-R "**res\_req**"** ...]
[options] command [arguments]

**bsub**** -pack **job_submission_file

**bsub** **-h**[**elp**] [**all**] [**description**]
[category_name ...] [**-**option_name ...]

**bsub** **-V**

# Categories and Options



Use the keyword all to display all options and the keyword
description to display a detailed description of the bsub
command. For more details on specific categories and options,
specify bsub -h with the name of the categories and options.

   Categories  
   List categories for options of the bsub command.

   Options  
   List of options for the bsub command.

   Description  
   Submits a job for execution and assigns it a unique numerical
   job ID.





**-a**



Specifies one or more application-specific esub or epsub
executable files that you want LSF to associate with the job.




# Categories



schedule



# Synopsis



**bsub -a "**application_name
[([argument[,argument...]])]...**"**


# Description



The value of -a must correspond to the application name of an
actual esub or epsub file. For example, to use bsub -a fluent,
one or both of the esub.fluent or epsub.fluent files must exist
in **LSF\_SERVERDIR**.

For example, to submit a job that invokes the
application-specific esub (or epsub) executables named
esub.license (or epsub.license) and esub.fluent (or
epsub.fluent), enter:

bsub -a "license fluent" my_job

The name of the application-specific esub or epsub program is
passed to the parent esub. The parent esub program
(LSF_SERVERDIR/mesub) handles job submission requirements of the
application. Application-specific esub programs can specify their
own job submission requirements. The value of -a is set in the
**LSB\_SUB\_ADDITIONAL** environment variable.

mesub uses the method name license to invoke the esub named
LSF_SERVERDIR/esub.license or the epsub named
LSF_SERVERDIR/epsub.license, and the method name fluent to invoke
the esub named LSF_SERVERDIR/esub.fluent or the epsub named
LSF_SERVERDIR/epsub.fluent. Therefore, mesub is run twice: Before
the job is submitted to mbatchd to run the esub scripts, and
after the job is submitted to run the epsub scripts.

LSF first invokes the executable file named esub (without
_.application\_name_ in the file name) if it exists in
**LSF\_SERVERDIR**, followed by any mandatory esub or epsub
executable files that are defined using the parameter
**LSB\_ESUB\_METHOD** in the lsf.conf file, and then any
application-specific esub executable files (with
_.application\_name_ in the file name) specified by -a. After
the job is submitted, LSF invokes the executable file named epsub
(without _.application\_name_ in the file name) if it exists
in **LSF\_SERVERDIR**, followed by any mandatory epsub
executable files (as specified using the parameter
**LSB\_ESUB\_METHOD**), and then any application-specific epsub
executable files (with _.application\_name_ in the file name)
specified by -a.

The name of the esub or epsub program must be a valid file name.
It can contain only alphanumeric characters, underscore (\_)
and hyphen (-).

**Restriction: **If the value of -a corresponds to a value in
**LSB\_ESUB\_METHOD**, this value must correspond to an actual
esub or epsub file in **LSF\_SERVERDIR**. For example, if
fluent is defined in **LSB\_ESUB\_METHOD**, the esub.fluent
or epsub.fluent file must exist in **LSF\_SERVERDIR** to use
bsub -a fluent.

If you have an esub or epsub that runs an interactive or X-window
job and you have SSH enabled in lsf.conf, the communication
between hosts is encrypted.

esub arguments provide flexibility for filtering and modifying
job submissions by letting you specify options for esub
executable files. epsub provides the flexibility to perform
external logic using information about jobs that are just
submitted, such as the assigned job ID and queue name.

The variables you define in the esub and epsub arguments can
include environment variables and command output substitution.

**Note: **The same arguments that are passed to esub are also
passed to epsub. You cannot pass different arguments to an esub
file and an epsub file with the same application name.

Valid esub and epsub arguments can contain alphanumeric
characters, spaces, special characters (\`"\$!) and other
characters (~@#%^&*()-=\_+[]|{};':,./&lt;&gt;?). Special patterns
like variables (for example, $PATH) and program output (for
example, \`ls\` command output) in an esub or epsub argument will
also be processed.

For example, if you use bsub -a “esub1 ($PATH, \`ls\`)” user_job,
the first argument passed to esub1 would be the value of variable
_PATH_, and the second argument passed to esub1 would be the
output of command ls.

You can include a special character in an esub or epsub argument
with an escape character or a pair of apostrophes (''). The usage
may vary among different shells. You can specify an argument
containing separators ('(', ')', ',') and space
characters ('').

You can also use an escape character (\) to specify
arguments containing special characters, separators and space
characters. For example:

bsub –a “application_name1(var1,var2 contain \()” user_job

For fault tolerance, extra space characters are allowed between
entities including esub/epsub, separators, and arguments. For
example, the following is valid input:

bsub -a “ esub1 ( var1 , var2 ) ” user_job

The maximum length allowed for an esub/epsub argument is 1024
characters. The maximum number of arguments allowed for an
esub/epsub is 128.

Jobs submitted with an esub (or epsub) will show all the esubs in
bjobs -l output, first with the default and then user esubs.


# Examples



*  To specify a single argument for a single esub/epsub
   executable file, use:

   bsub –a “application_name(var1)” user_job

*  To specify multiple arguments for a single esub/epsub
   executable file, use:

   bsub –a “application_name(var1,var2,...,varN)” user_job

*  To specify multiple arguments including a string argument for
   a single esub/epsub executable file, use:

   bsub –a “application_name(var1,var2 is a string,...,varN)”
   user_job

*  To specify arguments for multiple esub/epsub executable files,
   use:

   bsub –a “application_name1(var1,var2)
   application_name2(var1,var2)” user_job

*  To specify no argument to an esub/epsub executable file, use:

   bsub –a “application_name1” user_job

Parent topic: Options






**-alloc\_flags**



Specifies the user level CSM allocation prologs and epilogs.




# Categories



resource



# Synopsis



**bsub -alloc_flags "**flag1 [**flag2 ...]"**


# Description



This option is for LSF jobs that are submitted with the IBM
Cluster Systems Manager (CSM) integration package.

Specify an alphanumeric string of flags and separate multiple
flags with a space.

Parent topic: Options






**-app**



Submits the job to the specified application profile.




# Categories



properties



# Synopsis



**bsub -app **application_profile_name


# Description



You must specify an existing application profile. If the
application profile does not exist in lsb.applications, the job
is rejected.

Parent topic: Options






**-ar**



Specifies that the job is auto-resizable.




# Categories



properties



# Synopsis



**bsub -ar**

Parent topic: Options






**-B**



Sends mail to you when the job is dispatched and begins
execution.




# Categories



notify



# Synopsis



**bsub -B**

Parent topic: Options






**-b**



Dispatches the job for execution on or after the specified date
and time.




# Categories



schedule



# Synopsis



**bsub -b** [[year:][month**:**]day**:**]hour**:**minute


# Description



The date and time are in the form of
[[_year_:][_month_:]_day_:]_hour_:_minute_
where the number ranges are as follows: year after 1970, month
1-12, day 1-31, hour 0-23, minute 0-59.

At least two fields must be specified. These fields are assumed
to be hour:minute. If three fields are given, they are assumed to
be day:hour:minute, four fields are assumed to be
_month_:_day_:_hour_:_minute_,
and five fields are assumed to be
_year_:_month_:_day_:_hour_:_minute_.

If the year field is specified and the specified time is in the
past, the start time condition is considered reached and LSF
dispatches the job if slots are available.


# Examples



bsub -b 20:00 -J my_job_name my\_program

Submit my\_program to run after 8 p.m. and assign it
the job name my\_job\_name.

Parent topic: Options






**-C**



Sets a per-process (soft) core file size limit for all the
processes that belong to this job.




# Categories



limit



# Synopsis



**bsub -C **core_limit


# Description



For more information, see getrlimit(2).

By default, the limit is specified in KB. Use
**LSF\_UNIT\_FOR\_LIMITS** in lsf.conf to specify a larger unit
for the limit (MB, GB, TB, PB, or EB).

The behavior of this option depends on platform-specific UNIX or
Linux systems.

In some cases, the process is sent a SIGXFSZ signal if the job
attempts to create a core file larger than the specified limit.
The SIGXFSZ signal normally terminates the process.

In other cases, the writing of the core file terminates at the
specified limit.

Parent topic: Options






**-c**



Limits the total CPU time the job can use.




# Categories



limit



# Synopsis



**bsub -c** [hour**:**]minute[**/**host_name |
**/**host_model]


# Description



This option is useful for preventing runaway jobs or jobs that
use up too many resources. When the total CPU time for the whole
job has reached the limit, a SIGXCPU signal is first sent to the
job, then SIGINT, SIGTERM, and SIGKILL.

If **LSB\_JOB\_CPULIMIT** in lsf.conf is set to n, LSF-enforced
CPU limit is disabled and LSF passes the limit to the operating
system. When one process in the job exceeds the CPU limit, the
limit is enforced by the operating system.

The CPU limit is in the form of [_hour_:]_minute_.
The minutes can be specified as a number greater than 59. For
example, three and a half hours can either be specified as 3:30,
or 210.

The CPU time you specify is the _normalized_ CPU time. This
is done so that the job does approximately the same amount of
processing for a given CPU limit, even if it is sent to host with
a faster or slower CPU. Whenever a normalized CPU time is given,
the actual time on the execution host is the specified time
multiplied by the CPU factor of the normalization host then
divided by the CPU factor of the execution host.

Optionally, you can supply a host name or a host model name
defined in LSF. You must insert a slash (/) between the CPU
limit and the host name or model name. If a host name or model
name is not given, LSF uses the default CPU time normalization
host defined at the queue level (**DEFAULT\_HOST\_SPEC** in
lsb.queues) if it has been configured, otherwise uses the default
CPU time normalization host defined at the cluster level
(**DEFAULT\_HOST\_SPEC** in lsb.params) if it has been
configured, otherwise uses the submission host.

Jobs submitted to a chunk job queue are not chunked if the CPU
limit is greater than 30 minutes.


# Examples



bsub -q "queue1 queue2 queue3" -c 5 my\_program

Submit my\_program to one of the candidate queues:
queue1, queue2, and queue3 that are selected
according to the CPU time limit specified by -c 5.

Parent topic: Options






**-clusters**



LSF multicluster capability only. Specifies cluster names when
submitting jobs.




# Categories



resource, schedule



# Synopsis



**bsub -clusters** "**all** [**~**cluster_name] ... |
cluster\_name[**+**[pref_level]] ...
[**others**[**+**[pref_level]]]"


# Description



You can specify cluster names when submitting jobs for LSF
multicluster capability.

The -clusters option has the following keywords:

**all**  
         Specifies both local cluster and all remote clusters in
         the **SNDJOBS\_TO** parameter of the target queue in
         **lsb.queues**. For example:

         bsub -clusters all -q &lt;_send\_queue_&gt;

         LSF will go through the** SNDJOBS\_TO** parameter in
         lsb.queues to check whether asked clusters (except for
         the local cluster) are members of **SNDJOBS\_TO**. If
         any cluster except the local cluster does not exist in
         **SNDJOBS\_TO**, the job is rejected with an error
         message.

**others**  
         Sends the job to all clusters except for the clusters
         you specify. For example:

         bsub -clusters "c1+3 c2+1 others+2"

**~**  
         Must be used with all to indicate the rest of the
         clusters, excluding the specified clusters.

**+**  
         When followed by a positive integer, specifies job level
         preference for requested clusters. For example:

         bsub -clusters "c1+2 c2+1"

If the local cluster name is local_c1, and
**SNDJOBS\_TO**=q1@rmt_c1 q2@rmt_c2 q3@rmt\_c3, then the
requested cluster should be local\_c1 and rmt\_c3. For
example:

bsub -clusters "all ~rmt_c1 ~rmt\_c2"

-clusters local_cluster restricts the job for dispatch to local
hosts. To run a job on remote clusters only, use:

bsub -clusters "all ~local\_cluster"

A job that only specifies remote clusters will not run on local
hosts. Similarly, a job that only specifies local clusters will
not run on remote hosts. If a job specifies local and remote
clusters, the job tries local hosts first, then remote clusters.

If there are multiple default queues, then when bsub -clusters
remote\_clusters is issued, the job is sent to the queue whose
**SNDJOBS\_TO** contains the requested clusters. For example:

bsub -clusters "c2" , DEFAULT_QUEUE=q1 q2, q1:
SNDJOBS_TO=recvQ1@c1 recvQ2@c3, q2: SNDJOBS_TO=recvQ1@c1
recvQ2@c2

The job is sent to q2.

To have the job try to run on only local hosts:

bsub -q mc -clusters local\_c1

To have the job try to run on only remote hosts (e.g.,
rmt\_c1 and rmt\_c2):

bsub -q mc -clusters rmt_c1 rmt\_c2

To have the job first try local hosts, then rmt\_c1 and
rmt\_c2:

bsub -q mc -clusters local_c1 rmt_c1 rmt\_c2

To ignore preference for the local cluster (because the local
cluster is always tried first, even though remote clusters have
higher job level preference) and try remote clusters:

bsub -q mc -clusters local_c1 rmt_c1+2 rmt\_c2+1

To have the job first try the local cluster, then remote
clusters:

bsub -q mc -clusters all

To have the job first try the local cluster, then try all remote
clusters except for rmt\_c1:

bsub -q mc -clusters all ~rmt\_c1

To have the job try only remote clusters:

bsub -q mc -clusters all ~local\_c1

The -clusters option is supported in esub, so LSF administrators
can direct jobs to specific clusters if they want to implement
flow control. The -m option and -clusters option cannot be used
together.

Parent topic: Options






**-cn\_cu**



Specifies the cu (compute unit) resource requirement string for
the compute node for the CSM job.




# Categories



resource



# Synopsis



**bsub -cn\_cu** cu_string


# Conflicting Options



Do not use with the -csm y option.


# Description



This option is for easy mode LSF job submission to run with the
IBM Cluster Systems Manager (CSM) integration package.

Parent topic: Options






**-cn\_mem**



Specifies the memory limit that is required on the compute node
for the CSM job.




# Categories



resource



# Synopsis



**bsub -cn\_mem** mem_size


# Conflicting Options



Use only with the -csm y option.


# Description



This option is for expert mode LSF job submission to run with the
IBM Cluster Systems Manager (CSM) integration package.

Parent topic: Options






**-core\_isolation**



Disables or enables core isolation.




# Categories



resource



# Synopsis



**bsub -core_isolation 0** | **1** | **2** | **3** |
**4** | **5**


# Description



This option is for LSF jobs that are submitted with the IBM
Cluster Systems Manager (CSM) integration package.

Specify 0 to disable core isolation.

Specify any integer between 1 and 5 to enable core isolation.
This number is passed to CSM.


# Default



0. Core isolation is disabled.

Parent topic: Options






**-csm**



Enables expert mode for the CSM job submission, which allows you
to specify a full resource requirement string for CSM resources.




# Categories



properties



# Synopsis



**bsub -csm y** | **n**


# Description



This option is for LSF jobs that are submitted with the IBM
Cluster Systems Manager (CSM) integration package.


# Default



n. LSF uses easy mode when submitting jobs with CSM options.

Parent topic: Options






**-cwd**



Specifies the current working directory for job execution.




# Categories



io



# Synopsis



**bsub -cwd "**current\_working\_directory**"**


# Description



The system creates the current working directory (CWD) if the
path for the CWD includes dynamic patterns for both absolute and
relative paths. LSF cleans the created CWD based on the time to
live value set in the **JOB\_CWD\_TTL** parameter of the
application profile or in lsb.params.

The path can include the following dynamic patterns, which are
case sensitive:

*  %J - job ID

*  %JG - job group (if not specified, it will be ignored)

*  %I - index (default value is 0)

*  %EJ - execution job ID

*  %EI - execution index

*  %P - project name

*  %U - user name

*  %G - user group

*  %H - first execution host name

If the job is submitted with -app but without -cwd, and
**LSB\_JOB\_CWD** is not defined, then the application profile
defined **JOB\_CWD** will be used. If **JOB\_CWD** is not
defined in the application profile, then the
**DEFAULT\_JOB\_CWD** value is used.

In forwarding mode, if a job is not submitted with the -cwd
option and **LSB\_JOB\_CWD** is not defined, then **JOB\_CWD**
in the application profile or the **DEFAULT\_JOB\_CWD** value for
the execution cluster is used.

LSF does not allow environment variables to contain other
environment variables to be expanded on the execution side.

By default, if the current working directory is not accessible on
the execution host, the job runs in /tmp (on UNIX) or
c:\LSF_version\_num_\tmp (on Windows). If the environment
variable **LSB\_EXIT\_IF\_CWD\_NOTEXIST** is set to Y and the
current working directory is not accessible on the execution
host, the job exits with the exit code 2.


# Examples



The following command creates
/scratch/jobcwd/user1/&lt;jobid&gt;\_0/ for the job CWD:

bsub -cwd "/scratch/jobcwd/%U/%J_%I" myjob

The system creates submission\_dir/user1/&lt;jobid&gt;\_0/ for the
job's CWD with the following command:

bsub -cwd "%U/%J_%I" myprog

If the cluster wide CWD was defined and there is no default
application profile CWD defined:

**DEFAULT_JOB_CWD =/scratch/jobcwd/ %U/%J\_%I**

then the system creates: /scratch/jobcwd/user1/&lt;jobid&gt;\_0/
for the job's CWD.

Parent topic: Options






**-D**



Sets a per-process (soft) data segment size limit for each of the
processes that belong to the job.




# Categories



limit



# Synopsis



**bsub -D **data_limit


# Description



For more information, see getrlimit(2). The limit is specified in
KB.

This option affects calls to sbrk() and brk() . An
sbrk() or malloc() call to extend the data segment
beyond the data limit returns an error.

**Note: **Linux does not use sbrk() and brk()
within its calloc() and malloc(). Instead, it uses
(mmap()) to create memory. DATALIMIT cannot be enforced on
Linux applications that call sbrk() and malloc().

Parent topic: Options






**-data**



Specifies data requirements for a job.




# Categories



properties, resource



# Synopsis



**-data** **"**[host\_name**:**]abs_file_path
[[host\_name**:/**]abs_file_path ...]**"** ... [**-datagrp**
**"**user\_group\_name**"**] [**-datachk**] | **-data**
**"tag:**[tag\_owner**@**]tag_name
[**tag:**[tag\_owner**@**]tag_name ...]**"** ...

**-data**
**"**[host\_name**:**]abs\_folder\_path[**/**[<b>\*</b>]]
[[host\_name**:/**]abs\_folder\_path**/**[***] ...]"**
 ... [**-datagrp** **"**user\_group\_name**"**]
[**-datachk**] | **-data**
**"tag:**[tag\_owner**@**]tag_name
[**tag:**[tag\_owner**@**]tag_name ...]**"** ...


# Description



You can specify data requirements for the job three ways:

*  As a list of files for staging.

*  An absolute path to a folder that contains files for staging.

*  As a list of arbitrary tag names.

Your job can specify multiple -data options, but all the
requested data requirements must be either tags, files, or
folders. You can specify individual files, folders, or data
specification files in each -data clause. You cannot mix tag
names with file specifications in the same submission. The
combined requirement of all -data clauses, including the
requirements that are specified inside specification files, are
combined into a single space-separated string of file
requirements.

Valid file or tag names can contain only alphanumeric characters
([A-z|a-z|0-9]), and a period (.), underscore
(\_), and dash (-). The tag or file name cannot
contain the special operating system names for parent directory
(../), current directory (./), or user home directory
(~/). File, folder, and tag names cannot contain spaces.
Tag names cannot begin with a dash character.

A list of files must contain absolute paths to the required
source files. The list of files can refer to actual data files
your job requires or to a data specification file that contains a
list of the data files. The first line of a data specification
file must be #@dataspec.

A data requirement that is a folder must contain an absolute path
to the folder. The colon character (:) is supported in the
path of a job requirement.

A host name is optional. The default host name is the submission
host. The host name is the host that LSF attempts to stage the
required data file from. You cannot substitute IP addresses for
host names.

By default, if the **CACHE\_PERMISSIONS=group** parameter is
specified in lsf.datamanager, the data manager uses the primary
group of the user who submitted the job to control access to the
cached files. Only users that belong to that group can access the
files in the data requirement. Use the -datagrp option to specify
a particular user group for access to the cache. The user who
submits the job must be a member of the specified group. You can
specify only one -datagrp option per job.

The _tag\_name_ element is any string that can be used as a
valid data tag.

If the **CACHE_ACCESS_CONTROL = Y** parameter is configured in
the lsf.datamanager file, you can optionally specify a user name
for the tag owner tag:[_tag\_owner_@]_tag\_name_.

The sanity check for the existence of files or folders and
whether the user can access them, discovery of the size and
modification of the files or folders, and generation of the hash
from the bsub command occurs in the transfer job. This equalizes
submission performance between jobs with and without data
requirements. The -datachk option forces the bsub and bmod
commands to perform these operations. If the data requirement is
for a tag, the -datachk option has no effect.

You can use the %I runtime variable in file paths. The
variable resolves to an array index in a job array submission. If
the job is not an array, %I in the data specification is
interpreted as 0. You cannot use the %I variable in
tag names.

The path to the data requirement files or the files that are
listed inside data specification files can contain wildcard
characters. All paths that are resolved from wildcard characters
are interpreted as files to transfer. The path to a data
specification file cannot contain wildcard characters.

The use of wildcard characters has the following rules:

*  A path that ends in a slash followed by an asterisk (/*)
   is interpreted as a directory. For example, a path like
   /data/august/*. Transfers only the files in the august
   directory without recursion into subdirectories.

*  A path that ends in a slash (/) means that you want to
   transfer all of the files in the folder and all files in all
   of its sub folders, recursively.

*  You can use the asterisk (*) wildcard character only at
   the end of the path. For example, a path like
   /data/*/file\_*.* is not supported. The following path is
   correct: /data/august/*. The * is subject to
   expansion by the shell and must be quoted properly.

*  When wildcard characters are expanded, symbolic links in a
   path are expanded. If a broken symbolic link is detected, the
   job submission fails.

*  When you use the asterisk character at the end of the path,
   the data file requirements must be in quotation marks.


# Examples



The following job requests three data files for staging, listing
in the -data option:

bsub -o %J.out -data "hostA:/proj/std/model.tar   
hostA:/proj/user1/case02341.dat hostB:/data/human/seq342138.dna" /share/bin/job_copy.sh  
 Job &lt;1962&gt; is submitted to default queue &lt;normal&gt;.   


The following job requests the same data files for staging.
Instead of listing them individually in the -data option, the
required files are listed in a data specification file named
/tmp/dataspec.user1:

bsub -o %J.out -data “/tmp/dataspec.user1” /share/bin/job_copy.sh  
 Job &lt;1963&gt; is submitted to default queue &lt;normal&gt;.   


The data specification file /tmp/dataspec.user1 contains the
paths to the required files:

cat /tmp/dataspec.user1  
 #@dataspec  
 hostA:/proj/std/model.tar  
 hostA:/proj/user1/case02341.dat  
 hostB:/data/human/seq342138.dna  


The following job requests all data files in the folder
/proj/std/on hostA. Only the contents of the top level of
the folder are requested.

bsub -o %J.out -data "hostA:/proj/std/*" /share/bin/job_copy.sh  
 Job &lt;1964&gt; is submitted to default queue &lt;normal&gt;.   


The following job requests all data files in the folder
/proj/std/, including on hostA. All files in all subfolders
are requested recursively as the required data files.

bsub -o %J.out -data "hostA:/proj/std/" /share/bin/job_copy.sh  
 Job &lt;1964&gt; is submitted to default queue &lt;normal&gt;.   


The following command submits an array job that requests data
files by array index. All data files must exist.

bsub -o %J.out -J "A[1-10]" -data "hostA:/proj/std/input_%I.dat"   
/share/bin/job_copy.sh  
 Job &lt;1965&gt; is submitted to default queue &lt;normal&gt;.   


The following job requests data files by tag name.

bsub -o %J.out -J "A[1-10]" -data "tag:SEQ_DATA_READY" "tag:SEQ_DATA2"   
/share/bin/job_copy.sh  
 Job &lt;1966&gt; is submitted to default queue &lt;normal&gt;.   


The following job requests the data file /proj/std/model.tar,
which belongs to the user group design1:

bsub -o %J.out -data "hostA:/proj/std/model.tar" -datagrp "design1" my_job.sh  
 Job &lt;1962&gt; is submitted to default queue &lt;normal&gt;.   


Parent topic: Options






**-datachk**



For jobs with a data requirement, enables the bsub and bmod
commands to perform a full sanity check on the files and folders,
and to generate the hash for each file and folder.




# Categories



properties, resource



# Synopsis



**-data** [**-datachk**]


# Description



For jobs with a data requirement, this command option forces the
bsub and bmod commands to perform the following operations on the
files or folders:

*  Sanity check to see if the files or folders exist

*  Check whether the submission user can access the files or
   folders

*  Discover the size and modification times of the files or
   folders

*  Generate a hash for each file or folder

If you do not specify this command option, these operations occur
at the transfer job by default. You can also force the bsub and
bmod commands to perform these operations by setting the
LSF_DATA_BSUB_CHKSUM parameter to y or Y in the
lsf.conf file.

This option only works for jobs with a data requirement. If the
data requirement is for a tag, this option has no effect.

Parent topic: Options






**-datagrp**



Specifies user group for job data requirements.




# Categories



properties, resource



# Synopsis



[**-datagrp** **"**user_group_name **"**]


# Description



By default, if the **CACHE\_PERMISSIONS=group** parameter is
specified in lsf.datamanager, the data manager uses the primary
group of the user who submitted the job to control access to the
cached files. Only users who belong to that group can access the
files in the data requirement. Use the -datagrp option to specify
a particular user group for access to the cache. The user who
submits the job must be a member of the specified group. You can
specify only one -datagrp option per job.

The job must have a data requirement to use -datagrp.

Normally, if you submit a data job with the -datagrp option, you
must belong to the data group specified. If you do not belong to
the specified group, the mbatchd daemon rejects the job
submission. If the **LSF\_DATA\_SKIP\_GROUP\_CHECK=Y** parameter is
configured in the lsf.conf file, mbatchd does no user group
checking, and just accepts the job submission. The parameter
**LSF\_DATA\_SKIP\_GROUP\_CHECK=Y** affects only the mbatchd
daemon. User group checking is not affected by how the
**CACHE\_ACCESS\_CONTROL** parameter is configured in the
lsf.datamanager file.


# Examples



The following job requests the data file /proj/std/model.tar,
which belongs to the user group design1:

bsub -o %J.out -data "hostA:/proj/std/model.tar" -datagrp "design1" my_job.sh  
 Job &lt;1962&gt; is submitted to default queue &lt;normal&gt;.   


Parent topic: Options






**-E**



Runs the specified job-based pre-execution command on the
execution host before actually running the job.




# Categories



properties



# Synopsis



**bsub -E "**pre_exec_command [arguments ...]**"**


# Description



For a parallel job, the job-based pre-execution command runs on
the first host selected for the parallel job. If you want the
pre-execution command to run on a specific first execution host,
specify one or more first execution host candidates at the job
level using -m, at the queue level with **PRE\_EXEC** in
lsb.queues, or at the application level with **PRE\_EXEC** in
lsb.applications.

If the pre-execution command returns a zero (0) exit code, LSF
runs the job on the selected host. Otherwise, the job and its
associated pre-execution command goes back to PEND status and is
rescheduled. LSF keeps trying to run pre-execution commands and
pending jobs. After the pre-execution command runs successfully,
LSF runs the job. You must ensure that the pre-execution command
can run multiple times without causing side effects, such as
reserving the same resource more than once.

The standard input and output for the pre-execution command are
directed to the same files as the job. The pre-execution command
runs under the same user ID, environment, home, and working
directory as the job. If the pre-execution command is not in the
user's usual execution path (the $PATH variable), the full path
name of the command must be specified.

**Note: **The command path can contain up to 4094 characters
for UNIX and Linux, or up to 255 characters for Windows,
including the directory and file name.

Parent topic: Options






**-e**



Appends the standard error output of the job to the specified
file path.




# Categories



io



# Synopsis



**bsub -e **error_file


# Description



If the parameter **LSB\_STDOUT\_DIRECT** in lsf.conf is set to Y
or y, the standard error output of a job is written to the file
you specify as the job runs. If **LSB\_STDOUT\_DIRECT** is not
set, it is written to a temporary file and copied to the
specified file after the job finishes. **LSB\_STDOUT\_DIRECT** is
not supported on Windows.

If you use the special character %J in the name of the
error file, then %J is replaced by the job ID of the job.
If you use the special character %I in the name of the
error file, then %I is replaced by the index of the job in
the array if the job is a member of an array. Otherwise, %I
is replaced by 0 (zero).

If the current working directory is not accessible on the
execution host after the job starts, LSF writes the standard
error output file to /tmp/.

If the specified _error\_file_ path is not accessible, the
output will not be stored.

If the specified _error\_file_ is the same as the
_output\_file_ (as specified by the -o option), the
_error\_file_ of the job is NULL and the standard error of the
job is stored in the output file.

**Note: **The file path can contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows, including
the directory, file name, and expanded values for %J
(_job\_ID_) and %I (_index\_ID_).

Parent topic: Options






**-env**



Controls the propagation of the specified job submission
environment variables to the execution hosts.




# Categories



properties



# Synopsis



**bsub -env "none"** | **"****all, ** [**~**var_name[,
**~**var_name] ...] [var\_name**=**var_value[,
var\_name**=**var_value] ...]**"** |
**"**var\_name[**=**var_value][,
var\_name[**=**var_value] ...]**"**


# Description



Specify a comma-separated list of job submission environment
variables to control the propagation to the execution hosts.

*  Specify none with no other variables to submit jobs with
   no submission environment variables. All environment variables
   are removed while submitting the job.

*  Specify the variable name without a value to propagate the
   environment variable with its default value.

*  Specify the variable name with a value to propagate the
   environment variable with the specified value to overwrite the
   default value. The specified value may either be a new value
   or quote the value of an existing environment variable (unless
   you are submitting job packs). For example:

   In UNIX, fullpath=/tmp/:$filename appends /tmp/ to
   the beginning of the _filename_ environment variable and
   assigns this new value to the _fullpath_ environment
   variable. Use a colon (:) to separate multiple
   environment variables.

   In Windows, fullpath=\Temp​%filename% appends
   \Temp\ to the beginning of the _filename_
   environment variable and assigns this new value to the
   _fullpath_ environment variable. Use a semicolon (;)
   to separate multiple environment variables.

   The shell under which you submitted the job will parse the
   quotation marks.

*  Specify all at the beginning of the list to propagate
   all existing submission environment variables to the execution
   hosts. You may also assign values to specific environment
   variables.

   For example, -env "all, var1=value1, var2=value2"
   submits jobs with all the environment variables, but with the
   specified values for the _var1_ and _var2_ environment
   variables.

*  When using the all keyword, add ~ to the beginning
   of the variable name and the environment variable is not
   propagated to the execution hosts.

The environment variable names cannot contain the following words
and symbols: "none", "all", comma (,), tilde
(~), equals sign (=), double quotation mark (")
and single quotation mark (').

The variable value can contain a tilde (~) and a comma
(,). However, if the value contains a comma (,), the
entire value must be enclosed in single quotation marks. For
example:

bsub -env "TEST='A, B' "

An esub can change the -env environment variables by writing them
to the file specified by the **LSB\_SUB\_MODIFY\_FILE** or
**LSF\_SUB4\_SUB\_ENV\_VARS** environment variables. If both
environment variables are specified, **LSF\_SUB\_MODIFY\_FILE**
takes effect.

When -env is not specified with bsub, the default value is
-env "all" (that is, all environment variables are
submitted with the default values).

The entire argument for the -env option may contain a maximum of
4094 characters for UNIX and Linux, or up to 255 characters for
Windows.

If -env conflicts with -L, the value of -L takes effect.

The following environment variables are not propagated to
execution hosts because they are only used in the submission
host:

*  HOME, LS_JOBPID, LSB_ACCT_MAP, LSB_EXIT_PRE_ABORT,
   LSB_EXIT_REQUEUE, LSB_EVENT_ATTRIB, LSB_HOSTS,
   LSB_INTERACTIVE, LSB_INTERACTIVE_SSH, LSB_INTERACTIVE_TTY,
   LSB_JOBFILENAME, LSB_JOBGROUP, LSB_JOBID, LSB_JOBNAME,
   LSB_JOB_STARTER, LSB_QUEUE, LSB_RESTART, LSB_TRAPSIGS,
   LSB_XJOB_SSH, LSF_VERSION, PWD, USER, VIRTUAL_HOSTNAME, and
   all variables with starting with LSB_SUB_

*  Environment variables about non-interactive jobs: TERM,
   TERMCAP

*  Windows-specific environment variables: COMPUTERNAME, COMSPEC,
   NTRESKIT, OS2LIBPATH, PROCESSOR_ARCHITECTURE,
   PROCESSOR_IDENTIFIER, PROCESSOR_LEVEL, PROCESSOR_REVISION,
   SYSTEMDRIVE, SYSTEMROOT, TEMP, TMP

The following environment variables do not take effect on the
execution hosts: LSB_DEFAULTPROJECT, LSB_DEFAULT_JOBGROUP,
LSB_TSJOB_ENVNAME, LSB_TSJOB_PASSWD, LSF_DISPLAY_ALL_TSC,
LSF_JOB_SECURITY_LABEL, LSB_DEFAULT_USERGROUP,
LSB_DEFAULT_RESREQ, LSB_DEFAULTQUEUE, BSUB_CHK_RESREQ,
LSB_UNIXGROUP, LSB_JOB_CWD

Parent topic: Options






**-eo**



Overwrites the standard error output of the job to the specified
file path.




# Categories



io



# Synopsis



**bsub -eo **error_file


# Description



If the parameter **LSB\_STDOUT\_DIRECT** in lsf.conf is set to Y
or y, the standard error output of a job is written to the file
you specify as the job runs, which occurs every time the job is
submitted with the overwrite option, even if it is requeued
manually or by the system. If **LSB\_STDOUT\_DIRECT** is not set,
it is written to a temporary file and copied to the specified
file after the job finishes. **LSB\_STDOUT\_DIRECT** is not
supported on Windows.

If you use the special character %J in the name of the
error file, then %J is replaced by the job ID of the job.
If you use the special character %I in the name of the
error file, then %I is replaced by the index of the job in
the array if the job is a member of an array. Otherwise, %I
is replaced by 0 (zero).

If the current working directory is not accessible on the
execution host after the job starts, LSF writes the standard
error output file to /tmp/.

If the specified _error\_file_ path is not accessible, the
output will not be stored.

**Note: **The file path can contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows, including
the directory, file name, and expanded values for %J
(_job\_ID_) and %I (_index\_ID_).

Parent topic: Options






**-Ep**



Runs the specified job-based post-execution command on the
execution host after the job finishes.




# Categories



properties



# Synopsis



**bsub -Ep "**post_exec_command [arguments ...]**"**


# Description



If both application-level (**POST\_EXEC** in lsb.applications)
and job-level post-execution commands are specified, job level
post-execution overrides application-level post-execution
commands. Queue-level post-execution commands (**POST\_EXEC** in
lsb.queues) run after application-level post-execution and
job-level post-execution commands.

**Note: **The command path can contain up to 4094 characters
for UNIX and Linux, or up to 255 characters for Windows,
including the directory and file name.

Parent topic: Options






**-eptl**



Specifies the eligible pending time limit for the job.




# Categories



limit



# Synopsis



**bsub -eptl** [hour**:**]minute


# Description



TRACK\_ELIGIBLE\_PENDINFO=Y must be defined in the lsb.params
file to use the bsub -eptl option.

LSF sends the job-level eligible pending time limit configuration
to IBM Spectrum LSF RTM (LSF RTM), which handles the alarm and
triggered actions such as user notification (for example,
notifying the user that submitted the job and the LSF
administrator) and job control actions (for example, killing the
job). LSF RTM compares the job's eligible pending time to the
eligible pending time limit, and if the job is in an eligible
pending state for longer than this specified time limit, LSF RTM
triggers the alarm and actions. Without LSF RTM, LSF shows the
pending time limit and takes no action on the pending time limit.

In MultiCluster job forwarding mode, the job's eligible pending
time limit is ignored in the execution cluster, while the
submission cluster merges the job's queue-, application-, and
job-level eligible pending time limit according to local
settings.

The eligible pending time limit is in the form of
[_hour_:]_minute_. The minutes can be specified as
a number greater than 59. For example, three and a half hours can
either be specified as 3:30, or 210.

To specify a pending time limit or eligible pending time limit at
the queue or application level, specify the **PEND\_TIME\_LIMIT**
or **ELIGIBLE\_PEND\_TIME\_LIMIT** parameters in the lsb.queues or
lsb.applications files.

The job-level eligible pending time limit specified here
overrides any application-level or queue-level limits specified
(**ELIGIBLE\_PEND\_TIME\_LIMIT** in lsb.applications and
lsb.queues).

Parent topic: Options






**-ext**



Specifies application-specific external scheduling options for
the job.




# Categories



schedule



# Synopsis



**bsub -ext**[**sched**]
**"**external\_scheduler\_options**"**


# Description



To enable jobs to accept external scheduler options, set
LSF\_ENABLE\_EXTSCHEDULER=y in lsf.conf.

You can abbreviate the -extsched option to -ext.

You can specify only one type of external scheduler option in a
single -extsched string.

For example, Linux hosts and SGI VCPUSET hosts running CPUSET can
exist in the same cluster, but they accept different external
scheduler options. Use external scheduler options to define job
requirements for either Linux or CPUSET, but not both. Your job
runs either on Linux hosts or CPUSET hosts. If external scheduler
options are not defined, the job may run on a Linux host but it
does not run on a CPUSET host.

The options set by -extsched can be combined with the queue-level
**MANDATORY\_EXTSCHED** or **DEFAULT\_EXTSCHED** parameters. If
-extsched and **MANDATORY\_EXTSCHED** set the same option, the
**MANDATORY\_EXTSCHED** setting is used. If -extsched and
**DEFAULT\_EXTSCHED** set the same options, the -extsched
setting is used.

Use **DEFAULT\_EXTSCHED** in lsb.queues to set default external
scheduler options for a queue.

To make certain external scheduler options mandatory for all jobs
submitted to a queue, specify **MANDATORY\_EXTSCHED** in
lsb.queues with the external scheduler options you need or your
jobs.

Parent topic: Options






**-F**



Sets a per-process (soft) file size limit for each of the
processes that belong to the job.




# Categories



limit



# Synopsis



**bsub -F **file_limit


# Description



For more information, see getrlimit(2). The limit is specified in
KB.

If a job process attempts to write to a file that exceeds the
file size limit, then that process is sent a SIGXFSZ signal. The
SIGXFSZ signal normally terminates the process.

Parent topic: Options






**-f**



Copies a file between the local (submission) host and the remote
(execution) host.




# Categories



io



# Synopsis



**bsub -f "** local_file operator [remote\_file]**"** ...


# Description



Specify absolute or relative paths, including the file names. You
should specify the remote file as a file name with no path when
running in non-shared systems.

If the remote file is not specified, it defaults to the local
file, which must be given. Use multiple -f options to specify
multiple files.

**Note: **The file path can contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows, including
the directory and file name.

**operator**  
         An operator that specifies whether the file is copied to
         the remote host, or whether it is copied back from the
         remote host. The operator must be surrounded by white
         space.

         The following describes the operators:

         &gt; Copies the local file to the remote file before
         the job starts. Overwrites the remote file if it exists.

         &lt; Copies the remote file to the local file after
         the job completes. Overwrites the local file if it
         exists.

         &lt;&lt; Appends the remote file to the local file after
         the job completes. The local file must exist.

         &gt;&lt; Copies the local file to the remote file before
         the job starts. Overwrites the remote file if it exists.
         Then copies the remote file to the local file after the
         job completes. Overwrites the local file.

         &lt;&gt; Copies the local file to the remote file before
         the job starts. Overwrites the remote file if it exists.
         Then copies the remote file to the local file after the
         job completes. Overwrites the local file.

All of the above involve copying the files to the output
directory defined in **DEFAULT\_JOB\_OUTDIR** or with bsub
-outdir instead of the submission directory, as long as the path
is relative. The output directory is created at the start of the
job, and also applies to jobs that are checkpointed, migrated,
requeued or rerun.

If you use the -i _input_file _option, then you do not have
to use the -f option to copy the specified input file to the
execution host. LSF does this for you, and removes the input file
from the execution host after the job completes.

If you use the -o _out\_file_,-e _err\_file_, -oo
_out\_file_, or the -eo _err\_file_ option, and you want
the specified file to be copied back to the submission host when
the job completes, then you must use the -f option.

If the submission and execution hosts have different directory
structures, you must make sure that the directory where the
remote file and local file are placed exists.

If the local and remote hosts have different file name spaces,
you must always specify relative path names. If the local and
remote hosts do not share the same file system, you must make
sure that the directory containing the remote file exists. It is
recommended that only the file name be given for the remote file
when running in heterogeneous file systems. This places the file
in the job's current working directory. If the file is shared
between the submission and execution hosts, then no file copy is
performed.

LSF uses lsrcp to transfer files (see lsrcp(1) command). lsrcp
contacts RES on the remote host to perform the file transfer. If
RES is not available, rcp is used (see rcp(1)). The user must
make sure that the rcp binary is in the user's $PATH on the
execution host.

Jobs that are submitted from LSF client hosts should specify the
-f option only if rcp is allowed. Similarly, rcp must be allowed
if account mapping is used.

Parent topic: Options






**-freq**



Specifies a CPU frequency for a job.




# Categories



resource



# Synopsis



**bsub -freq** numberUnit


# Description



The submission value will overwrite the application profile value
and the application profile value will overwrite the queue value.
The value is float and should be specified with SI units (GHz,
MHz, KHz). If no units are specified GHz is the default.

Parent topic: Options






**-G**



For fair share scheduling. Associates the job with the specified
group.




# Categories



schedule



# Synopsis



**bsub -G **user_group


# Description



Specify any group that you belong to. You must be a direct member
of the specified user group.

If **ENFORCE\_ONE\_UG\_LIMITS** is enabled in lsb.params, using
the -G option enforces any limits placed on the specified user
group only even if the user or user group belongs to more than
one group .

If **ENFORCE\_ONE\_UG\_LIMITS** is disabled in lsb.params
(default), using the -G option enforces the strictest limit that
is set on any of the groups that the user or user group belongs
to.

Parent topic: Options






**-g**



Submits jobs in the specified job group.




# Categories



properties



# Synopsis



**bsub -g **job_group_name


# Description



The job group does not have to exist before submitting the job.
For example:

bsub -g /risk_group/portfolio1/current myjob  
Job &lt;105&gt; is submitted to default queue.  


Submits myjob to the job group
/risk\_group/portfolio1/current.

If group /risk\_group/portfolio1/current exists, job 105 is
attached to the job group.

Job group names can be up to 512 characters long.

If group /risk\_group/portfolio1/current does not exist, LSF
checks its parent recursively, and if no groups in the hierarchy
exist, all three job groups are created with the specified
hierarchy and the job is attached to group.

You can use -g with -sla. All jobs in a job group attached to a
service class are scheduled as SLA jobs. It is not possible to
have some jobs in a job group not part of the service class.
Multiple job groups can be created under the same SLA. You can
submit additional jobs to the job group without specifying the
service class name again. You cannot use job groups with
resource-based SLAs that have guarantee goals.

For example, the following attaches the job to the service class
named opera, and the group /risk\_group/portfolio1/current:

bsub -sla opera -g /risk_group/portfolio1/current myjob  


To submit another job to the same job group, you can omit the SLA
name:

bsub -g /risk_group/portfolio1/current myjob2  


Parent topic: Options






**-gpu**



Specifies properties of GPU resources required by the job.




# Categories



resources



# Synopsis



**bsub -gpu -** | **"**[**num=**num\_gpus[**/****task**
| **host**]] [**:mode=shared** | **exclusive\_process**]
[**:mps=****yes**[**,shared**][**,nocvd**] | **no**]
[**:j\_exclusive=****yes** | **no**] [**:aff=****yes** |
**no**] [**:block=****yes** | **no**]
[**:gpack=****yes** | **no**] [**:gvendor=amd** |
**nvidia**] [**:gmodel=**model\_name[**-**mem\_size]]
[**:gtile=**tile\_num|**'!'**] [**:gmem=**mem\_value]
[**:glink=yes**] [**:mig=**GI\_size[**/**CI\_size]]**"**


# Description



**-**  
         Specifies that the job does not set job-level GPU
         requirements. Use the hyphen with no letter to set the
         effective GPU requirements, which are defined at the
         cluster, queue, or application profile level.

         If a GPU requirement is specified at the cluster, queue,
         and application profile level, each option (num, mode,
         mps, j_exclusive, gmodel, gtile, gmem, and nvlink) of
         the GPU requirement is merged separately. Application
         profile level overrides queue level, which overrides the
         cluster level default GPU requirement.

         If there are no GPU requirements defined in the cluster,
         queue, or application level, the default value is
         "num=1:mode=shared:mps=no:j\_exclusive=no".

**num=num_gpus[/task | host]**  
         The number of physical GPUs required by the job. By
         default, the number is per host. You can also specify
         that the number is per task by specifying /task
         after the number.

         If you specified that the number is per task, the
         configuration of the ngpus\_physical resource in
         the lsb.resources file is set to PER\_TASK, or the
         **RESOURCE\_RESERVE\_PER\_TASK=Y** parameter is set in
         the lsb.params file, this number is the requested count
         per task.

**mode=shared | exclusive\_process**  
         The GPU mode when the job is running, either shared or
         exclusive_process. The default mode is shared.

         The shared mode corresponds to the Nvidia or AMD DEFAULT
         compute mode. The exclusive_process mode corresponds to
         the Nvidia EXCLUSIVE_PROCESS compute mode.

         **Note: ** Do not specify exclusive_process when you
         are using AMD GPUs (that is, when gvendor=amd is
         specified).

**mps=yes[,nocvd][,shared] | no**  
         Enables or disables the Nvidia Multi-Process Service
         (MPS) for the GPUs that are allocated to the job. Using
         MPS effectively causes the EXCLUSIVE_PROCESS mode to
         behave like the DEFAULT mode for all MPS clients. MPS
         always allows multiple clients to use the GPU through
         the MPS server.

         **Note: **To avoid inconsistent behavior, do not
         enable mps when you are using AMD GPUs (that is, when
         gvendor=amd is specified). If the result of merging the
         GPU requirements at the cluster, queue, application, and
         job levels is gvendor=amd and mps is enabled (for
         example, if gvendor=amd is specified at the job level
         without specifying mps=no, but mps=yes is specified at
         the application, queue, or cluster level), LSF ignores
         the mps requirement.

         MPS is useful for both shared and exclusive process
         GPUs, and allows more efficient sharing of GPU resources
         and better GPU utilization. See the Nvidia documentation
         for more information and limitations.

         When using MPS, use the EXCLUSIVE_PROCESS mode to ensure
         that only a single MPS server is using the GPU, which
         provides additional insurance that the MPS server is the
         single point of arbitration between all CUDA process for
         that GPU.

         You can also enable MPS daemon sharing by adding the
         **share** keyword with a comma and no space (for
         example, mps=yes,shared enables MPS daemon sharing
         on the host). If sharing is enabled, all jobs that are
         submitted by the same user with the same resource
         requirements share the same MPS daemon on the host,
         socket, or GPU.

         **Important: **Using EXCLUSIVE_THREAD mode with MPS is
         not supported and might cause unexpected behavior.

**j_exclusive=yes | no**  
         Specifies whether the allocated GPUs can be used by
         other jobs. When the mode is set to exclusive_process,
         the j_exclusive=yes option is set automatically.

**block=yes | no**  
         Specifies whether to enable block distribution, that is,
         to distribute the allocated GPUs of a job as blocks when
         the number of tasks is greater than the requested number
         of GPUs. If set to yes, LSF distributes all the
         allocated GPUs of a job as blocks when the number of
         tasks is bigger than the requested number of GPUs. By
         default, block=no is set so that allocated GPUs
         are not distributed as blocks.

         For example, if a GPU job requests to run on a host with
         4 GPUs and 40 tasks, block distribution assigns GPU0 for
         ranks 0-9, GPU1 for ranks 10-19, GPU2 for tanks 20-29,
         and GPU3 for ranks 30-39.

         **Note: **The block=yes setting conflicts with
         aff=yes (strict CPU-GPU affinity binding). This is
         because strict CPU-GPU binding allocates GPUs to tasks
         based on the CPU NUMA ID, which conflicts with the
         distribution of allocated GPUs as blocks. If
         block=yes and aff=yes are both specified in
         the GPU requirements string, the block=yes setting
         takes precedence and strict CPU-GPU affinity binding is
         disabled (that is, aff=no is automatically set).

**gpack=yes | no**  
         For shared mode jobs only. Specifies whether to enable
         pack scheduling. If set to yes, LSF packs multiple
         shared mode GPU jobs to allocated GPUs. LSF schedules
         shared mode GPUs as follows:

         1. LSF sorts the candidate hosts (from largest to
            smallest) based on the number of shared GPUs that
            already have running jobs, then by the number of GPUs
            that are not exclusive.

            If the order[] keyword is defined in the resource
            requirements string, after sorting order[], LSF
            re-sorts the candidate hosts by the gpack policy (by
            shared GPUs that already have running jobs first,
            then by the number of GPUs that are not exclusive).
            The gpack policy sort priority is higher than the
            order[] sort.

         2. LSF sorts the candidate GPUs on each host (from
            largest to smallest) based on the number of running
            jobs.

         After scheduling, the shared mode GPU job packs to the
         allocated shared GPU that is sorted first, not to a new
         shared GPU.

         If Docker attribute affinity is enabled, the order of
         candidate hosts are sorted by Docker attribute affinity
         before sorting by GPUs.

         By default, gpack=no is set so that pack
         scheduling is disabled.

**gvendor=amd | nvidia**  
         Specifies the GPU vendor type. LSF allocates GPUs with
         the specified vendor type.

         Specify amd to request AMD GPUs, or specify
         nvidia to request Nvidia GPUs.

         By default, LSF requests Nvidia GPUs.

**gmodel=model\_name[-mem\_size]**  
         Specifies GPUs with the specific model name and,
         optionally, its total GPU memory. By default, LSF
         allocates the GPUs with the same model, if available.

         The **gmodel** keyword supports the following formats:

         **gmodel=model\_name**  
                  Requests GPUs with the specified brand and
                  model name (for example, TeslaK80).

         **gmodel=short\_model\_name**  
                  Requests GPUs with a specific brand name (for
                  example, Tesla, Quadro, NVS, ) or model type
                  name (for example, K80, P100).

         **gmodel=model\_name-mem\_size**  
                  Requests GPUs with the specified brand name and
                  total GPU memory size. The GPU memory size
                  consists of the number and its unit, which
                  includes M, G, T, MB,
                  GB, and TB (for example,
                  12G).

         To find the available GPU model names on each host, run
         the lsload –gpuload, lshosts –gpu, or bhosts -gpu
         commands. The model name string does not contain space
         characters. In addition, the slash (/) and hyphen
         (-) characters are replaced with the underscore
         character (\_). For example, the GPU model name
         “Tesla C2050 / C2070” is converted to
         “TeslaC2050\_C2070” in LSF.

**gmem=mem\_value**  
         Specify the GPU memory on each GPU required by the job.
         The format of _mem\_value_ is the same to other
         resource value (for example, mem or swap) in
         the rusage section of the job resource requirements
         (-R).

**gtile=! | tile\_num**  
         Specifies the number of GPUs per socket. Specify an
         number to explicitly define the number of GPUs per
         socket on the host, or specify an exclamation mark
         (!) to enable LSF to automatically calculate the
         number, which evenly divides the GPUs along all sockets
         on the host. LSF guarantees the **gtile** requirements
         even for affinity jobs. This means that LSF might not
         allocate the GPU's affinity to the allocated CPUs when
         the **gtile** requirements cannot be satisfied.

         If the **gtile** keyword is not specified for an
         affinity job, LSF attempts to allocate enough GPUs on
         the sockets that allocated GPUs. If there are not enough
         GPUs on the optimal sockets, jobs cannot go to this
         host.

         If the **gtile** keyword is not specified for a
         non-affinity job, LSF attempts to allocate enough GPUs
         on the same socket. If this is not available, LSF might
         allocate GPUs on separate GPUs.

**nvlink=yes**  
         Obsolete in LSF, Version 10.1 Fix Pack 11. Use the
         **glink** keyword instead. Enables the job enforcement
         for NVLink connections among GPUs. LSF allocates GPUs
         with NVLink connections in force.

**glink=yes**  
         Enables job enforcement for special connections among
         GPUs. LSF must allocate GPUs with the special
         connections that are specific to the GPU vendor.

         If the job requests AMD GPUs, LSF must allocate GPUs
         with the xGMI connection. If the job requests Nvidia
         GPUs, LSF must allocate GPUs with the NVLink connection.

         Do not use **glink** together with the obsolete
         **nvlink** keyword.

         By default, LSF can allocate GPUs without special
         connections when there are not enough GPUs with these
         connections.

**mig=GI\_size[/CI\_size]**  
         Specifies Nvidia Multi-Instance GPU (MIG) device
         requirements.

         Specify the requested number of GPU instances for the
         MIG job. Valid GPU instance sizes are 1, 2, 3, 4, 7.

         Optionally, specify the requested number of compute
         instances after the specified GPU instance size and a
         slash character (/). The requested compute
         instance size must be less than or equal to the
         requested GPU instance size. In addition, Nvidia MIG
         does not support the following GPU/compute instance size
         combinations: 4/3, 7/5, 7/6. If this is not specified,
         the default compute instance size is 1.

The syntax of the GPU requirement in the -gpu option is the same
as the syntax in the **LSB\_GPU\_REQ** parameter in the lsf.conf
file and the **GPU\_REQ** parameter in the lsb.queues and
lsb.applications files.

**Note: **The bjobs output does not show aff=yes even if
you specify aff=yes in the bsub -gpu option.

If the **GPU\_REQ\_MERGE** parameter is defined as Y or
y in the lsb.params file and a GPU requirement is specified
at multiple levels (at least two of the default cluster, queue,
application profile, or job level requirements), each option of
the GPU requirement is merged separately. Job level overrides
application level, which overrides queue level, which overrides
the default cluster GPU requirement. For example, if the mode
option of the GPU requirement is defined on the -gpu option, and
the mps option is defined in the queue, the mode of job level and
the mps value of queue is used.

If the **GPU\_REQ\_MERGE** parameter is not defined as Y or
y in the lsb.params file and a GPU requirement is specified
at multiple levels (at least two of the default cluster, queue,
application profile, or job level requirements), the entire GPU
requirement string is replaced. The entire job level GPU
requirement string overrides application level, which overrides
queue level, which overrides the default GPU requirement.

The esub parameter **LSB\_SUB4\_GPU\_REQ** modifies the value of
the -gpu option.

LSF selects the GPU that meets the topology requirement first. If
the GPU mode of the selected GPU is not the requested mode, LSF
changes the GPU to the requested mode. For example, if LSF
allocates an exclusive\_process GPU to a job that needs a
shared GPU, LSF changes the GPU mode to shared before the job
starts and then changes the mode back to exclusive\_process
when the job finishes.

The GPU requirements are converted to rusage resource
requirements for the job. For example, num=2 is converted
to rusage[ngpus\_physical=2]. Use the bjobs, bhist, and
bacct commands to see the merged resource requirement.

There might be complex GPU requirements that the bsub -gpu option
and **GPU\_REQ** parameter syntax cannot cover, including
compound GPU requirements (for different GPU requirements for
jobs on different hosts, or for different parts of a parallel
job) and alternate GPU requirements (if more than one set of GPU
requirements might be acceptable for a job to run). For complex
GPU requirements, use the bsub -R command option, or the
**RES\_REQ** parameter in the lsb.applications or lsb.queues
file to define the resource requirement string.

**Important: **You can define the mode, j_exclusive, and mps
options only with the -gpu option, the **LSB\_GPU\_REQ**
parameter in the lsf.conf file, or the **GPU\_REQ** parameter in
the lsb.queues or lsb.applications files. You cannot use these
options with the rusage resource requirement string in the bsub
-R command option or the **RES\_REQ** parameter in the
lsb.queues or lsb.applications files.


# Examples



The following job request does not override the effective GPU
requirement with job-level GPU requirements. The effective GPU
requirement is merged together from GPU requirements that are
specified at the cluster, queue, and application profile level.
Application profile level overrides queue level, which overrides
the cluster level default GPU requirement.

bsub -gpu - ./app

The following job requires 2 EXCLUSIVE\_PROCESS mode GPUs
and starts MPS before running the job.

bsub -gpu "num=2:mode=exclusive_process:mps=yes" ./app

The following job requires 2 DEFAULT mode GPUs and uses
them exclusively. The two GPUs cannot be used by other jobs even
though the mode is shared.

bsub -gpu "num=2:mode=shared:j_exclusive=yes" ./app

The following job uses 3 DEFAULT mode GPUs and shares them
with other jobs.

bsub -gpu "num=3:mode=shared:j_exclusive=no" ./app

The following job uses 4 EXCLUSIVE\_PROCESS GPUs that cannot
be used by other jobs. The j_exclusive option defaults to yes for
this job.

bsub -gpu "num=4:mode=exclusive_process" ./app

The following job requires two tasks. Each task requires 2
EXCLUSIVE\_PROCESS GPUs on two hosts. The GPUs are allocated
in the same NUMA as the allocated CPU.

bsub -gpu "num=2:mode=exclusive_process" -n2 -R "span[ptile=1] affinity[core(1)]" ./app

The following job uses 2 Nvidia MIG GPUs with 3 GPU instances and
2 compute instances.

bsub -gpu "num=2:mig=3/2" ./app


# See Also



**LSB\_GPU\_REQ** parameter in the lsf.conf file and the
**GPU\_REQ** parameter in the lsb.queues and lsb.applications
files

Parent topic: Options






**-H**



Holds the job in the PSUSP state when the job is submitted.




# Categories



schedule



# Synopsis



**bsub -H**


# Description



The job is not scheduled until you tell the system to resume the
job (see bresume(1)).

Parent topic: Options






**Categories**



List categories for options of the bsub command.

   Category: io  
   Specify input or output files and directories: -cwd, -e, -eo,
   -f, -i, -is, -o, -oo, -outdir, -tty.

   Category: limit  
   Specify limits: -c, -C, -D, -eptl, -F, -hl, -M, -p, -ptl, -S,
   -T, -ul, -v, -W, -We.

   Category: notify  
   Control user notification: -B, -K, -N, -u.

   Category: pack  
   Submit job packs (single files containing multiple job
   requests): -pack.

   Category: properties  
   Specify job submission properties: -app, -ar, -csm, -data,
   -datagrp, -E, -env, -Ep, -g, -I, -Ip, -Is, -IS, -ISp, -ISs,
   -IX, -J, -Jd, -jsdl, -jsdl_strict, -jsm, -json, -k, -K, -L,
   -P, -q, -Q, -r, -rn, -rnc, -s, -sla, , -sp, -stage,
   -step_cgroup, -wa, -wt, -XF, -yaml, -Zs.

   Category: resource  
   Specify resources and resource requirements: -alloc_flags,
   -clusters, -cn_cu, -cn_mem, -core_isolation, -data, -datagrp,
   -freq, -gpu, -hostfile, -ln_mem, -ln_slots, -m, -n, -nnodes,
   -network, -R, -U.

   Category: schedule  
   Control job scheduling and dispatch: -a, -b, -clusters, -ext,
   -G, -H, -jobaff, -Lp, -mig, -t, -ti, -U, -w, -x.

   Category: script  
   Use job scripts: -Zs.

Parent topic: bsub






**Options**



List of options for the bsub command.

   -a  
   Specifies one or more application-specific esub or epsub
   executable files that you want LSF to associate with the job.

   -alloc_flags  
   Specifies the user level CSM allocation prologs and epilogs.

   -app  
   Submits the job to the specified application profile.

   -ar  
   Specifies that the job is auto-resizable.

   -B  
   Sends mail to you when the job is dispatched and begins
   execution.

   -b  
   Dispatches the job for execution on or after the specified
   date and time.

   -C  
   Sets a per-process (soft) core file size limit for all the
   processes that belong to this job.

   -c  
   Limits the total CPU time the job can use.

   -clusters  
   LSF multicluster capability only. Specifies cluster names when
   submitting jobs.

   -cn_cu  
   Specifies the cu (compute unit) resource requirement string
   for the compute node for the CSM job.

   -cn_mem  
   Specifies the memory limit that is required on the compute
   node for the CSM job.

   -core_isolation  
   Disables or enables core isolation.

   -csm  
   Enables expert mode for the CSM job submission, which allows
   you to specify a full resource requirement string for CSM
   resources.

   -cwd  
   Specifies the current working directory for job execution.

   -D  
   Sets a per-process (soft) data segment size limit for each of
   the processes that belong to the job.

   -data  
   Specifies data requirements for a job.

   -datachk  
   For jobs with a data requirement, enables the bsub and bmod
   commands to perform a full sanity check on the files and
   folders, and to generate the hash for each file and folder.

   -datagrp  
   Specifies user group for job data requirements.

   -E  
   Runs the specified job-based pre-execution command on the
   execution host before actually running the job.

   -Ep  
   Runs the specified job-based post-execution command on the
   execution host after the job finishes.

   -e  
   Appends the standard error output of the job to the specified
   file path.

   -env  
   Controls the propagation of the specified job submission
   environment variables to the execution hosts.

   -eo  
   Overwrites the standard error output of the job to the
   specified file path.

   -eptl  
   Specifies the eligible pending time limit for the job.

   -ext  
   Specifies application-specific external scheduling options for
   the job.

   -F  
   Sets a per-process (soft) file size limit for each of the
   processes that belong to the job.

   -f  
   Copies a file between the local (submission) host and the
   remote (execution) host.

   -freq  
   Specifies a CPU frequency for a job.

   -G  
   For fair share scheduling. Associates the job with the
   specified group.

   -g  
   Submits jobs in the specified job group.

   -gpu  
   Specifies properties of GPU resources required by the job.

   -H  
   Holds the job in the PSUSP state when the job is
   submitted.

   -hl  
   Enables job-level host-based memory and swap limit enforcement
   on systems that support Linux cgroups.

   -hostfile  
   Submits a job with a user-specified host file.

   -I  
   Submits an interactive job.

   -Ip  
   Submits an interactive job and creates a pseudo-terminal when
   the job starts.

   -IS  
   Submits an interactive job under a secure shell (ssh).

   -ISp  
   Submits an interactive job under a secure shell (ssh) and
   creates a pseudo-terminal when the job starts.

   -ISs  
   Submits an interactive job under a secure shell (ssh) and
   creates a pseudo-terminal with shell mode support when the job
   starts.

   -Is  
   Submits an interactive job and creates a pseudo-terminal with
   shell mode when the job starts.

   -IX  
   Submits an interactive X-Window job.

   -i  
   Gets the standard input for the job from specified file path.

   -is  
   Gets the standard input for the job from the specified file
   path, but allows you to modify or remove the input file before
   the job completes.

   -J  
   Assigns the specified name to the job, and, for job arrays,
   specifies the indices of the job array and optionally the
   maximum number of jobs that can run at any given time.

   -Jd  
   Assigns the specified description to the job; for job arrays,
   specifies the same job description for all elements in the job
   array.

   -jobaff  
   Specifies the affinity preferences for the job.

   -jsdl  
   Submits a job using a JSDL file that uses the LSF extension to
   specify job submission options.

   -jsdl_strict  
   Submits a job using a JSDL file that only uses the standard
   JSDL elements and POSIX extensions to specify job submission
   options.

   -jsm  
   Enables or disables the IBM Job Step Manager (JSM) daemon for
   the job.

   -json  
   Submits a job using a JSON file to specify job submission
   options.

   -K  
   Submits a job and waits for the job to complete. Sends job
   status messages to the terminal.

   -k  
   Makes a job checkpointable and specifies the checkpoint
   directory.

   -L  
   Initializes the execution environment using the specified
   login shell.

   -Lp  
   Assigns the job to the specified LSF License Scheduler
   project.

   -ln_mem  
   Specifies the memory limit on the launch node (LN) host for
   the CSM job.

   -ln_slots  
   Specifies the number of slots to use on the launch node (LN)
   host for the CSM job.

   -M  
   Sets a memory limit for all the processes that belong to the
   job.

   -m  
   Submits a job to be run on specific hosts, host groups, or
   compute units.

   -mig  
   Specifies the migration threshold for checkpointable or
   re-runnable jobs, in minutes.

   -N  
   Sends the job report to you by mail when the job finishes.

   -Ne  
   Sends the job report to you by mail when the job exits (that
   is, when the job is in Exit status).

   -n  
   Submits a parallel job and specifies the number of tasks in
   the job.

   -notify  
   Requests that the user be notified when the job reaches any of
   the specified states.

   -network  
   For LSF IBM Parallel Environment (IBM PE) integration.
   Specifies the network resource requirements to enable
   network-aware scheduling for IBM PE jobs.

   -nnodes  
   Specifies the number of compute nodes that are required for
   the CSM job.

   -o  
   Appends the standard output of the job to the specified file
   path.

   -oo  
   Overwrites the standard output of the job to the specified
   file path.

   -outdir  
   Creates the job output directory.

   -P  
   Assigns the job to the specified project.

   -p  
   Sets the limit of the number of processes to the specified
   value for the whole job.

   -pack  
   Submits job packs instead of an individual job.

   -ptl  
   Specifies the pending time limit for the job.

   -Q  
   Specify automatic job re-queue exit values.

   -q  
   Submits the job to one of the specified queues.

   -R  
   Runs the job on a host that meets the specified resource
   requirements.

   -r  
   Reruns a job if the execution host or the system fails; it
   does not rerun a job if the job itself fails.

   -rcacct  
   Assigns an account name (tag) to hosts that are borrowed
   through LSF resource connector, so that the hosts cannot be
   used by other user groups, users, or jobs.

   -rn  
   Specifies that the job is never re-runnable.

   -rnc  
   Specifies the full path of an executable to be invoked on the
   first execution host when the job allocation has been modified
   (both shrink and grow).

   -S  
   Sets a per-process (soft) stack segment size limit for each of
   the processes that belong to the job.

   -s  
   Sends the specified signal when a queue-level run window
   closes.

   -sla  
   Specifies the service class where the job is to run.

   -sp  
   Specifies user-assigned job priority that orders jobs in a
   queue.

   -stage  
   Specifies the options for direct data staging (for example,
   IBM CAST burst buffer).

   -step_cgroup  
   Enables the job to create a cgroup for each job step.

   -T  
   Sets the limit of the number of concurrent threads to the
   specified value for the whole job.

   -t  
   Specifies the job termination deadline.

   -ti  
   Enables automatic orphan job termination at the job level for
   a job with a dependency expression (set using -w).

   -tty  
   When submitting an interactive job, displays output/error
   messages on the screen (except pre-execution output/error
   messages).

   -U  
   If an advance reservation has been created with the brsvadd
   command, the job makes use of the reservation.

   -u  
   Sends mail to the specified email destination.

   -ul  
   Passes the current operating system user shell limits for the
   job submission user to the execution host.

   -v  
   Sets the total process virtual memory limit to the specified
   value for the whole job.

   -W  
   Sets the runtime limit of the job.

   -We  
   Specifies an estimated run time for the job.

   -w  
   LSF does not place your job unless the dependency expression
   evaluates to TRUE.

   -wa  
   Specifies the job action to be taken before a job control
   action occurs.

   -wt  
   Specifies the amount of time before a job control action
   occurs that a job warning action is to be taken.

   -XF  
   Submits a job using SSH X11 forwarding.

   -x  
   Puts the host running your job into exclusive execution mode.

   -yaml  
   Submits a job using a YAML file to specify job submission
   options.

   -Zs  
   Spools a job command file to the directory specified by the
   **JOB\_SPOOL\_DIR** parameter in lsb.params, and uses the
   spooled file as the command file for the job.

   command  
   Specifies the command and arguments used for the job
   submission.

   job_script  
   Specifies the job script for the bsub command to load, parse,
   and run from the command line.

   -h  
   Displays a description of the specified category, command
   option, or sub-option to stderr and exits.

   -V  
   Prints LSF release version to stderr and exits.

Parent topic: bsub






**-hl**



Enables job-level host-based memory and swap limit enforcement on
systems that support Linux cgroups.




# Categories



limit



# Synopsis



**bsub -hl**


# Description



When -hl is specified, a memory limit specified at the job level
by -M or by **MEMLIMIT** in lsb.queues or lsb.applications is
enforced by the Linux cgroup subsystem on a per-job basis on each
host. Similarly, a swap limit specified at the job level by -v or
by **SWAPLIMIT** in lsb.queues or lsb.applications is enforced
by the Linux cgroup subsystem on a per-job basis on each host.
Host-based memory and swap limits are enforced regardless of the
number of tasks running on the execution host. The -hl option
only applies to memory and swap limits; it does not apply to any
other resource usage limits.

**LSB\_RESOURCE\_ENFORCE="memory"** must be specified in lsf.conf
for host-based memory and swap limit enforcement with the -hl
option to take effect. If no memory or swap limit is specified
for the job (the merged limit for the job, queue, and application
profile, if specified), or **LSB\_RESOURCE\_ENFORCE="memory"** is
not specified, a host-based memory limit is not set for the job.

When **LSB\_RESOURCE\_ENFORCE="memory"** is configured in
lsf.conf, and memory and swap limits are specified for the job,
but -hl is _not_ specified, memory and swap limits are
calculated and enforced as a multiple of the number of tasks
running on the execution host.

Parent topic: Options






**-hostfile**



Submits a job with a user-specified host file.




# Categories



resource



# Synopsis



**bsub -hostfile** file_path


# Conflicting Options



Do not use with the following options: -ext, -n, -m, -R
_res-req_.


# Description



When submitting a job, you can point the job to a file that
specifies hosts and number of slots for job processing.

For example, some applications (typically when benchmarking) run
best with a very specific geometry. For repeatability (again,
typically when benchmarking) you may want it to always run it on
the same hosts, using the same number of slots.

The user-specified host file specifies a host and number of slots
to use per task, resulting in a rank file.

The -hostfile option allows a user to submit a job, specifying
the path of the user-specified host file:

bsub -hostfile "spec_host_file"

**Important: **

*  Do not use a user-specified host file if you have enabled task
   geometry as it may cause conflicts and jobs may fail.

*  Alternatively, if resources are not available at the time that
   a task is ready a job may not run smoothly. Consider using
   advance reservation instead of a user-specified host file, to
   ensure reserved slots are available.

Any user can create a user-specified host file. It must be
accessible by the user from the submission host. It lists one
host per line. The format is as follows:

# This is a user-specified host file  
&lt;host_name1&gt;   [&lt;# slots&gt;]  
&lt;host_name2&gt;   [&lt;# slots&gt;]  
&lt;host_name1&gt;   [&lt;# slots&gt;]  
&lt;host_name2&gt;   [&lt;# slots&gt;]  
&lt;host_name3&gt;   [&lt;# slots&gt;]  
&lt;host_name4&gt;   [&lt;# slots&gt;]

The following rules apply to the user-specified host file:

*  Insert comments starting with the # character.

*  Specifying the number of slots for a host is optional. If no
   slot number is indicated, the default is 1.

*  A host name can be either a host in a local cluster or a host
   leased-in from a remote cluster
   (_host\_name_@_cluster\_name_).

*  A user-specified host file should contain hosts from the same
   cluster only.

*  A host name can be entered with or without the domain name.

*  Host names may be used multiple times and the order entered
   represents the placement of tasks. For example:

   #first three tasks  
   host01                      3  
   #fourth tasks  
   host02  
   #next three tasks  
   host03                      3  


The resulting rank file is made available to other applications
(such as MPI).

The **LSB\_DJOB\_RANKFILE** environment variable is generated
from the user-specified host file. If a job is not submitted with
a user-specified host file then **LSB\_DJOB\_RANKFILE** points to
the same file as **LSB\_DJOB\_HOSTFILE**.

The esub parameter **LSB\_SUB4\_HOST\_FILE** reads and modifies
the value of the -hostfile option.

The following is an example of a user-specified host file that
includes duplicate host names:

user1: cat ./user1_host_file  
# This is my user-specified host file for job242  
host01   3  
host02      
host03   3  
host01      
host02   2  


This user-specified host file tells LSF to allocate 10 slots in
total (4 slots on host01, 3 slots on host02, and 3
slots on host03). Each line represents the order of task
placement.

Duplicate host names are combined, along with the total number of
slots for a host name and the results are used for scheduling
(whereas **LSB\_DJOB\_HOSTFILE** groups the hosts together) and
for **LSB\_MCPU\_HOSTS**. **LSB\_MCPU\_HOSTS** represents the job
allocation.

The result is the following:

**LSB\_DJOB\_RANKFILE:**

host01  
host01  
host01  
host02  
host03  
host03  
host03  
host01  
host02  
host02

**LSB\_DJOB\_HOSTFILE:**

host01  
host01  
host01  
host01  
host02  
host02  
host02  
host03  
host03  
host03

LSB_MCPU_HOSTS = host01 4 host02 3 host03 3

Parent topic: Options






**-I**



Submits an interactive job.




# Categories



properties



# Synopsis



**bsub -I [-tty]**


# Conflicting Options



Do not use with the following options: -Ip, -IS, -ISp, -ISs, -Is,
-IX, -K.


# Description



Submits an interactive job. A new job cannot be submitted until
the interactive job is completed or terminated.

Sends the job's standard output (or standard error) to the
terminal. Does not send mail to you when the job is done unless
you specify the -N option.

If the -i _input\_file_ option is specified, you cannot
interact with the job's standard input via the terminal.

If the -o _out\_file_ option is specified, sends the job's
standard output to the specified output file. If the -e
_err\_file_ option is specified, sends the job's standard
error to the specified error file_._

If used with -tty, also displays output/error (except pre-exec
output/error) on the screen.

Interactive jobs cannot be checkpointed.

Interactive jobs are not rerunnable (bsub -r).


# Examples



bsub -I ls

Submit an interactive job that displays the output of ls at the
user's terminal.

Parent topic: Options






**-i**



Gets the standard input for the job from specified file path.




# Categories



io



# Synopsis



**bsub -i **input_file


# Conflicting Options



Do not use with the -is option.


# Description



Specify an absolute or relative path. The input file can be any
type of file, though it is typically a shell script text file.

You can use the special characters %J and %I in the
name of the input file. %J is replaced by the job ID.
%I is replaced by the index of the job in the array, if the
job is a member of an array, otherwise by 0 (zero).

**Note: **The file path can contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows, including
the directory, file name, and expanded values for %J
(_job\_ID_) and %I (_index\_ID_).

If the file exists on the execution host, LSF uses it. Otherwise,
LSF attempts to copy the file from the submission host to the
execution host. For the file copy to be successful, you must
allow remote copy (rcp) access, or you must submit the job from a
server host where RES is running. The file is copied from the
submission host to a temporary file in the directory specified by
the **JOB\_SPOOL\_DIR** parameter in lsb.params, or your
$HOME/.lsbatch directory on the execution host. LSF removes this
file when the job completes.

By default, the input file is spooled to
LSB\_SHAREDIR/_cluster\_name_/lsf_indir. If the lsf_indir
directory does not exist, LSF creates it before spooling the
file. LSF removes the spooled file when the job completes. Use
the -is option if you need to modify or remove the input file
before the job completes. Removing or modifying the original
input file does not affect the submitted job.

**JOB\_SPOOL\_DIR** can be any valid path up to a maximum length
up to 4094 characters on UNIX and Linux or up to 255 characters
for Windows.

**JOB\_SPOOL\_DIR** must be readable and writable by the job
submission user, and it must be shared by the management host and
the submission host. If the specified directory is not accessible
or does not exist, bsub cannot write to the default directory
LSB\_SHAREDIR/_cluster\_name_/lsf_indir and the job fails.

Parent topic: Options






**-Ip**



Submits an interactive job and creates a pseudo-terminal when the
job starts.




# Categories



properties



# Synopsis



**bsub -Ip [-tty]**


# Conflicting Options



Do not use with the following options: -I, -IS, -ISp, -ISs, -Is,
-IX, -K.


# Description



Some applications (for example, vi) require a pseudo-terminal in
order to run correctly.

Options that create a pseudo-terminal are not supported on
Windows. Since the -Ip option creates a pseudo-terminal, it is
not supported on Windows.

A new job cannot be submitted until the interactive job is
completed or terminated.

Sends the job's standard output (or standard error) to the
terminal. Does not send mail to you when the job is done unless
you specify the -N option.

If the -i _input_file _option is specified, you cannot
interact with the job's standard input via the terminal.

If the -o _out\_file_ option is specified, sends the job's
standard output to the specified output file. If the -e
_err\_file_ option is specified, sends the job's standard
error to the specified error file_._

If used with -tty, also displays output/error (except pre-exec
output/error) on the screen.

Interactive jobs cannot be checkpointed.

Interactive jobs are not rerunnable (bsub -r).


# Examples



bsub -Ip vi myfile

Submit an interactive job to edit myfile.

Parent topic: Options






**-IS**



Submits an interactive job under a secure shell (ssh).




# Categories



properties



# Synopsis



**bsub -IS [-tty]**


# Conflicting Options



Do not use with the following options: -I, -Ip, -ISp, -ISs, -Is,
-IX, -K.


# Description



A new job cannot be submitted until the interactive job is
completed or terminated.

Sends the job's standard output (or standard error) to the
terminal. Does not send mail to you when the job is done unless
you specify the -N option.

If the -i _input_file _option is specified, you cannot
interact with the job's standard input via the terminal.

If the -o _out\_file_ option is specified, sends the job's
standard output to the specified output file. If the -e
_err\_file_ option is specified, sends the job's standard
error to the specified error file_._

If used with -tty, also displays output/error on the screen.

Interactive jobs cannot be checkpointed.

Interactive jobs are not rerunnable (bsub -r).

Parent topic: Options






**-Is**



Submits an interactive job and creates a pseudo-terminal with
shell mode when the job starts.




# Categories



properties



# Synopsis



**bsub -Is [-tty]**


# Conflicting Options



Do not use with the following options: -I, -Ip, -IS, -ISp, -ISs,
-IX, -K.


# Description



Some applications (for example, vi) require a pseudo-terminal in
order to run correctly.

Options that create a pseudo-terminal are not supported on
Windows. Since the -Is option creates a pseudo-terminal, it is
not supported on Windows.

Specify this option to add shell mode support to the
pseudo-terminal for submitting interactive shells, or
applications that redefine the CTRL-C and CTRL-Z keys (for
example, jove).

A new job cannot be submitted until the interactive job is
completed or terminated.

Sends the job's standard output (or standard error) to the
terminal. Does not send mail to you when the job is done unless
you specify the -N option.

If the -i _input_file _option is specified, you cannot
interact with the job's standard input via the terminal.

If the -o _out\_file_ option is specified, sends the job's
standard output to the specified output file. If the -e
_err\_file_ option is specified, sends the job's standard
error to the specified error file_._

If used with -tty, also displays output/error (except pre-exec
output/error) on the screen.

Interactive jobs cannot be checkpointed.

Interactive jobs are not rerunnable (bsub -r).


# Examples



bsub -Is csh

Submit an interactive job that starts csh as an interactive
shell.

Parent topic: Options






**-is**



Gets the standard input for the job from the specified file path,
but allows you to modify or remove the input file before the job
completes.




# Categories



io



# Synopsis



**bsub -is** input_file


# Conflicting Options



Do not use with the -i option.


# Description



Specify an absolute or relative path. The input file can be any
type of file, though it is typically a shell script text file.

The special characters %J and %I are not valid with
the -is option.

**Note: **The file path can contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows, including
the directory and file name.

If the file exists on the execution host, LSF uses it. Otherwise,
LSF attempts to copy the file from the submission host to the
execution host. For the file copy to be successful, you must
allow remote copy (rcp) access, or you must submit the job from a
server host where RES is running. The file is copied from the
submission host to a temporary file in the directory specified by
the **JOB\_SPOOL\_DIR** parameter in lsb.params, or your
$HOME/.lsbatch directory on the execution host. LSF removes this
file when the job completes.

By default, the input file is spooled to
LSB\_SHAREDIR/_cluster\_name_/lsf_indir. If the lsf_indir
directory does not exist, LSF creates it before spooling the
file. LSF removes the spooled file when the job completes. The
-is option allows you to modify or remove the input file before
the job completes. Removing or modifying the original input file
does not affect the submitted job.

If **JOB\_SPOOL\_DIR** is specified, the -is option spools the
input file to the specified directory and uses the spooled file
as the input file for the job.

**JOB\_SPOOL\_DIR** can be any valid path up to a maximum length
up to 4094 characters on UNIX and Linux or up to 255 characters
for Windows.

**JOB\_SPOOL\_DIR** must be readable and writable by the job
submission user, and it must be shared by the management host and
the submission host. If the specified directory is not accessible
or does not exist, bsub cannot write to the default directory
LSB\_SHAREDIR/_cluster\_name_/lsf_indir and the job fails.

Parent topic: Options






**-ISp**



Submits an interactive job under a secure shell (ssh) and creates
a pseudo-terminal when the job starts.




# Categories



properties



# Synopsis



**bsub -ISp [-tty]**


# Conflicting Options



Do not use with the following options: -I, -Ip, -IS, -ISs, -Is,
-IX, -K.


# Description



Some applications (for example, vi) require a pseudo-terminal in
order to run correctly. The options that create a pseudo-terminal
are not supported on Windows.

A new job cannot be submitted until the interactive job is
completed or terminated.

Sends the job's standard output (or standard error) to the
terminal. Does not send mail to you when the job is done unless
you specify the -N option.

If the -i _input_file _option is specified, you cannot
interact with the job's standard input via the terminal.

If the -o _out\_file_ option is specified, sends the job's
standard output to the specified output file. If the -e
_err\_file_ option is specified, sends the job's standard
error to the specified error file_._

If used with -tty, also displays output/error on the screen.

Interactive jobs cannot be checkpointed.

Interactive jobs are not rerunnable (bsub -r).

Parent topic: Options






**-ISs**



Submits an interactive job under a secure shell (ssh) and creates
a pseudo-terminal with shell mode support when the job starts.




# Categories



properties



# Synopsis



**bsub -ISs [-tty]**


# Conflicting Options



Do not use with the following options: -I, -Ip, -IS, -ISp, -Is,
-IX, -K.


# Description



Some applications (for example, vi) require a pseudo-terminal in
order to run correctly. The options that create a pseudo-terminal
are not supported on Windows.

Specify this option to add shell mode support to the
pseudo-terminal for submitting interactive shells, or
applications that redefine the CTRL-C and CTRL-Z keys (for
example, jove).

A new job cannot be submitted until the interactive job is
completed or terminated.

Sends the job's standard output (or standard error) to the
terminal. Does not send mail to you when the job is done unless
you specify the -N option.

If the -i _input_file _option is specified, you cannot
interact with the job's standard input via the terminal.

If the -o _out\_file_ option is specified, sends the job's
standard output to the specified output file. If the -e
_err\_file_ option is specified, sends the job's standard
error to the specified error file_._

If used with -tty, also displays output/error on the screen.

Interactive jobs cannot be checkpointed.

Interactive jobs are not rerunnable (bsub -r).

Parent topic: Options






**-IX**



Submits an interactive X-Window job.




# Categories



properties



# Synopsis



**bsub -IX [-tty]**


# Conflicting Options



Do not use with the following options: -I, -Ip, -IS, -ISp, -ISs,
-Is, -K.


# Description



A new job cannot be submitted until the interactive job is
completed or terminated.

Sends the job's standard output (or standard error) to the
terminal. Does not send mail to you when the job is done unless
you specify the -N option.

The session between X-client and X-server is encrypted; the
session between the execution host and submission host is also
encrypted. The following must be satisfied:

*  openssh must be installed and sshd must be running on the
   X-server

*  xhost + localhost or xhost +
   displayhost.domain.com on the X-server

*  ssh must be configured to run without a password or passphrase
   ($HOME/.ssh/authorized_keys must be set up)

   **Note: **In most cases ssh can be configured to run without
   a password by copying id_rsa.pub as authorized_keys with
   permission 600 (-rw-r--r--). Test by manually running ssh
   host.domain.com between the two hosts both ways and confirm
   there are no prompts using fully qualified host names.

If the -i _input_file _option is specified, you cannot
interact with the job's standard input via the terminal.

If the -o _out\_file_ option is specified, sends the job's
standard output to the specified output file. If the -e
_err\_file_ option is specified, sends the job's standard
error to the specified error file_._

If used with -tty, also displays output/error on the screen.

Interactive jobs cannot be checkpointed.

Interactive jobs are not rerunnable (bsub -r).


# Troubleshooting



Use the SSH command on the job execution host to connect it
securely with the job submission host. If the host fails to
connect, you can perform the following steps to troubleshoot.

*  Check the SSH version on both hosts. If the hosts have
   different SSH versions, a message is displayed identifying a
   protocol version mismatch.

*  Check that public and private key pairs are correctly
   configured.

*  Check the domain name.

   $ ssh –f –L 6000:localhost:6000 domain_name.example.com date   
   $ ssh –f –L 6000:localhost:6000 domain_name date

If these commands return errors, troubleshoot the domain name
with the error information returned.

The execution host should connect without passwords and pass
phrases; for example:

$ ssh sahpia03  
$ ssh sahpia03.example.com  


Parent topic: Options






**-J**



Assigns the specified name to the job, and, for job arrays,
specifies the indices of the job array and optionally the maximum
number of jobs that can run at any given time.




# Categories



properties



# Synopsis



**bsub -J **job_name | -J
"job\_name**[**index\_list**]%**job\_slot\_limit**"**


# Description



The job name does not need to be unique and can contain up to
4094 characters.

To specify a job array, enclose the index list in square
brackets, as shown, and enclose the entire job array
specification in quotation marks, as shown. The index list is a
comma-separated list whose elements have the syntax
[start-end[:step]] where start, end and step are positive
integers. If the step is omitted, a step of one is assumed. By
default, the job array index starts at one.

By default, the maximum number of jobs in a job array is 1000,
which means the maximum size of a job array (that is, the maximum
job array index) can never exceed 1000 jobs.

To change the maximum job array value, set
**MAX\_JOB\_ARRAY\_SIZE** in lsb.params to any positive integer
between 1 and 2147483646. The maximum number of jobs in a job
array cannot exceed the value set by **MAX\_JOB\_ARRAY\_SIZE**.

You may also use a positive integer to specify the system-wide
job slot limit (the maximum number of jobs that can run at any
given time) for this job array.

All jobs in the array share the same job ID and parameters. Each
element of the array is distinguished by its array index.

After a job is submitted, you use the job name to identify the
job. Specify "_job\_ID_[_index_]" to work with elements of
a particular array. Specify "_job\_name_[_index_]" to work
with elements of all arrays with the same name. Since job names
are not unique, multiple job arrays may have the same name with a
different or same set of indices.


# Examples



bsub -b 20:00 -J my_job_name my\_program

Submit my\_program to run after 8 p.m. and assign it
the job name my\_job\_name.

Parent topic: Options






**-Jd**



Assigns the specified description to the job; for job arrays,
specifies the same job description for all elements in the job
array.



# Categories



properties



# Synopsis



**bsub -Jd "**job_description"


# Description



The job description does not need to be unique and can contain up
to 4094 characters.

After a job is submitted, you can use bmod -Jd to change the job
description for any specific job array element, if required.

Parent topic: Options






**-jobaff**



Specifies the affinity preferences for the job.




# Categories



schedule



# Synopsis



**bsub -jobaff "**[**!** | **#** | **~**]attribute_name
 ...**"**

**bsub -jobaff "attribute(**[**!** | **#** |
**~**]attribute_name ...**)"**

**bsub -jobaff "**[**!** | **#** |
**~**]**samehost(**job\_id**)"**

**bsub -jobaff "**[**!** | **#** |
**~**]**samecu(**job\_id**)"**


# Description



The -jobaff command option uses one of the following keywords to
specify the affinity preferences for the job:

**[! | # | ~]attribute_name ...**  
         Use no keywords to specify the preferences for hosts on
         which LSF would run the current job based on host
         attributes. Use a space to separate multiple attributes.
         Specifying the following special characters before the
         attribute name indicates the following preferences:

         *  Default (no special character): It is preferred for
            the host to have this attribute.

         *  !: It is preferred for the host to not have
            this attribute.

         *  #: It is mandatory for the host to have this
            attribute.

         *  ~: It is mandatory for the host to not have
            this attribute.

**attribute([! | # | ~]attribute_name ...)**  
         Using the **attribute** keyword is the same as using
         no keywords (that is, using the **attribute** keyword
         specifies the preferences for hosts on which LSF would
         run the current job based on host attributes).

**[! | # | ~]samehost(job\_id)**  
         Use the **samehost** keyword to specify the preference
         for the job to run on the same host on which another job
         with the specified job ID runs. The job ID can be a
         simple job or array job element, but you cannot specify
         multiple job IDs. The **SAME\_JOB\_AFFINITY** parameter
         must be set to Y or y in the lsb.params file
         to use the **samehost** keyword.

         Specifying the following special characters before the
         **samehost** keyword indicates the following
         preferences:

         *  Default (no special character): It is preferred for
            the job to run on the same host on which the
            specified job runs.

         *  !: It is preferred for the job to not run on
            the same host on which the specified job runs.

         *  #: It is mandatory for the job to run on the
            same host on which the specified job runs.

         *  ~: It is mandatory for the job to not run on
            the same host on which the specified job runs.

**[! | # | ~]samecu(job\_id)**  
         Use the **samecu** keyword to specify the preference
         for the job to run on a compute unit on which another
         job with the specified job ID runs. The job ID can be a
         simple job or array job element, but you cannot specify
         multiple job IDs. The **SAME\_JOB\_AFFINITY** parameter
         must be set to Y or y in the lsb.params file
         to use the **samecu** keyword.

         Specifying the following special characters before the
         **samecu** keyword indicates the following
         preferences:

         *  Default (no special character): It is preferred for
            the job to run on the same compute unit on which the
            specified job runs.

         *  !: It is preferred for the job to not run on
            the same compute unit on which the specified job
            runs.

         *  #: It is mandatory for the job to run on the
            same compute unit on which the specified job runs.

         *  ~: It is mandatory for the job to not run on
            the same compute unit on which the specified job
            runs.

**Note: **If you submit a job with a **samehost** or
**samecu** affinity for a job that is finished and there are no
other unfinished jobs that reference this finished job at
submission time, LSF cleans the finished job's allocation host
information from the job scheduler. With no host allocation
information for the finished job, your job submission with
mandatory **samehost** or **samecu** requirements would pend
indefinitely, and if your job submission has a preferred
**samehost** or **samecu** requirement, the preferred
requirement is ignored.

Use the battr create command to create attributes on hosts.


# See Also



battr

Parent topic: Options






**-jsdl**



Submits a job using a JSDL file that uses the LSF extension to
specify job submission options.




# Categories



properties



# Synopsis



**bsub -jsdl** file_name


# Conflicting Options



Do not use with the following options: -json, -jsdl_strict,
-yaml.


# Description



LSF provides an extension to the JSDL specification so that you
can submit jobs using LSF features not defined in the JSDL
standard schema. The JSDL schema (jsdl.xsd), the POSIX extension
(jsdl-posix.xsd), and the LSF extension (jsdl-lsf.xsd) are
located in the LSF_LIBDIR directory.

*  To submit a job that uses the LSF extension, use the -jsdl
   option.

*  To submit a job that uses only standard JSDL elements and
   POSIX extensions, use the -jsdl_strict option. You can use the
   -jsdl_strict option to verify that your file contains only
   valid JSDL elements and POSIX extensions. Error messages
   indicate invalid elements, including:

   *  Elements that are not part of the JSDL specification

   *  Valid JSDL elements that are not supported in this version
      of LSF

   *  Extension elements that are not part of the JSDL standard
      and POSIX extension schemas

For more information about submitting jobs using JSDL, including
a detailed mapping of JSDL elements to LSF submission options,
and a complete list of supported and unsupported elements, refer
to Submitting jobs using JSDL in Administering IBM Spectrum LSF.

If you specify duplicate or conflicting job submission
parameters, LSF resolves the conflict by applying the following
rules:

1. The parameters that are specified in the command line override
   all other parameters.

2. The parameters that are specified in the JSDL file override
   the job script.

Parent topic: Options






**-jsdl\_strict**



Submits a job using a JSDL file that only uses the standard JSDL
elements and POSIX extensions to specify job submission options.




# Categories



properties



# Synopsis



**bsub -jsdl\_strict** file_name


# Conflicting Options



Do not use with the following options: -json, -jsdl, -yaml.


# Description



LSF provides an extension to the JSDL specification so that you
can submit jobs using LSF features not defined in the JSDL
standard schema. The JSDL schema (jsdl.xsd), the POSIX extension
(jsdl-posix.xsd), and the LSF extension (jsdl-lsf.xsd) are
located in the LSF_LIBDIR directory.

*  To submit a job that uses only standard JSDL elements and
   POSIX extensions, use the -jsdl_strict option. You can use the
   -jsdl_strict option to verify that your file contains only
   valid JSDL elements and POSIX extensions. Error messages
   indicate invalid elements, including:

   *  Elements that are not part of the JSDL specification

   *  Valid JSDL elements that are not supported in this version
      of LSF

   *  Extension elements that are not part of the JSDL standard
      and POSIX extension schemas

*  To submit a job that uses the LSF extension, use the -jsdl
   option.

For more information about submitting jobs using JSDL, including
a detailed mapping of JSDL elements to LSF submission options,
and a complete list of supported and unsupported elements, refer
to Submitting jobs using JSDL in Administering IBM Spectrum LSF.

If you specify duplicate or conflicting job submission
parameters, LSF resolves the conflict by applying the following
rules:

1. The parameters that are specified in the command line override
   all other parameters.

2. The parameters that are specified in the JSDL file override
   the job script.

Parent topic: Options






**-jsm**



Enables or disables the IBM Job Step Manager (JSM) daemon for the
job.




# Categories



properties



# Synopsis



**bsub -jsm y** | **n** | **d**


# Description



This option is for LSF jobs that are submitted with the IBM
Cluster Systems Manager (CSM) integration package, and defines if
the job is a JSM job using the jsrun mechanism.

If set to d, enables the JSM daemon in debug mode.


# Default



y. JSM daemons are enabled by default.

Parent topic: Options






**-json**



Submits a job using a JSON file to specify job submission
options.




# Categories



properties



# Synopsis



**bsub -json** file_name


# Conflicting Options



Do not use with the following options: -jsdl, -jsdl_strict,
-yaml.


# Description



In the JSON file, specify the bsub option name or alias and the
value as the key-value pair. To specify job command or job
script, use the command option name with the name of the
command or job script as the value. For options that have no
values (flags), use null or (for string-type options) an
empty value. Specify the key-value pairs under the category name
of the option.

If you specify duplicate or conflicting job submission
parameters, LSF resolves the conflict by applying the following
rules:

1. The parameters that are specified in the command line override
   all other parameters.

2. The parameters that are specified in the JSON file override
   the job script.


# Job Submission Options and Aliases



The following is a list of the bsub options to use in the file.
You can use either the option name without a hyphen, or the
alias. For example, to use the bsub -app option, specify either
**appName** or **app** as the key name, and the application
profile name as the key value.

**Table 1. List of supported bsub options and aliases**

+---------------+---------------+---------------+---------------+
| Option        | Alias         | Category      | Type          |
+---------------+---------------+---------------+---------------+
| a             | appSpecific   | schedule      | String        |
+---------------+---------------+---------------+---------------+
| alloc_flags   | allocFlags    | resource      | String        |
+---------------+---------------+---------------+---------------+
| app           | appName       | properties    | String        |
+---------------+---------------+---------------+---------------+
| ar            | autoResize    | properties    | String        |
+---------------+---------------+---------------+---------------+
| B             | notifyJobDisp | notify        | String        |
|               | atch          |               |               |
+---------------+---------------+---------------+---------------+
| b             | specifiedStar | schedule      | String        |
|               | tTime         |               |               |
+---------------+---------------+---------------+---------------+
| C             | coreLimit     | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| c             | cpuTimeLimit  | limit         | String        |
+---------------+---------------+---------------+---------------+
| clusters      | clusters      | resource      | String        |
|               |               | schedule      |               |
+---------------+---------------+---------------+---------------+
| cn_cu         | computeNodeCo | resource      | String        |
|               | mputeUnit     |               |               |
+---------------+---------------+---------------+---------------+
| cn_mem        | computeNodeMe | resource      | Numeric       |
|               | m             |               |               |
+---------------+---------------+---------------+---------------+
| core_isolatio | coreIsolation | resource      | Numeric       |
| n             |               |               |               |
+---------------+---------------+---------------+---------------+
| csm           | csm           | properties    | String        |
+---------------+---------------+---------------+---------------+
| cwd           | cwd           | io            | String        |
+---------------+---------------+---------------+---------------+
| D             | dataLimit     | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| data          | data          | resource      | String array  |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| datachk       | dataCheck     | resource      | String        |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| datagrp       | dataGroup     | resource      | String        |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| E             | preExecCmd    | properties    | String        |
+---------------+---------------+---------------+---------------+
| Ep            | psotExecCmd   | properties    | String        |
+---------------+---------------+---------------+---------------+
| e             | errorAppendFi | io            | String        |
|               | le            |               |               |
+---------------+---------------+---------------+---------------+
| env           | envVariable   | properties    | String        |
+---------------+---------------+---------------+---------------+
| eo            | errorOverwrit | io            | String        |
|               | eFile         |               |               |
+---------------+---------------+---------------+---------------+
| eptl          | epLimitRemain | limit         | String        |
+---------------+---------------+---------------+---------------+
| ext           | extSched      | schedule      | String        |
+---------------+---------------+---------------+---------------+
| F             | fileLimit     | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| f             | file          | io            | String        |
+---------------+---------------+---------------+---------------+
| freq          | frequency     | resource      | Numeric       |
+---------------+---------------+---------------+---------------+
| G             | userGroup     | schedule      | String        |
+---------------+---------------+---------------+---------------+
| g             | jobGroupName  | properties    | String        |
+---------------+---------------+---------------+---------------+
| gpu           | gpu           | resource      | String        |
+---------------+---------------+---------------+---------------+
| H             | hold          | schedule      | String        |
+---------------+---------------+---------------+---------------+
| hl            | hostLimit     | limit         | String        |
+---------------+---------------+---------------+---------------+
| hostfile      | hostFile      | resource      | String        |
+---------------+---------------+---------------+---------------+
| I             | interactive   | properties    | String        |
+---------------+---------------+---------------+---------------+
| Ip            | interactivePt | properties    | String        |
|               | y             |               |               |
+---------------+---------------+---------------+---------------+
| IS            | interactiveSs | properties    | String        |
|               | h             |               |               |
+---------------+---------------+---------------+---------------+
| ISp           | interactiveSs | properties    | String        |
|               | hPty          |               |               |
+---------------+---------------+---------------+---------------+
| ISs           | interactiveSs | properties    | String        |
|               | hPtyShell     |               |               |
+---------------+---------------+---------------+---------------+
| Is            | interactivePt | properties    | String        |
|               | yShell        |               |               |
+---------------+---------------+---------------+---------------+
| IX            | interactiveXW | properties    | String        |
|               | in            |               |               |
+---------------+---------------+---------------+---------------+
| i             | inputFile     | io            | String        |
+---------------+---------------+---------------+---------------+
| is            | inputHandleFi | io            | String        |
|               | le            |               |               |
+---------------+---------------+---------------+---------------+
| J             | jobName       | properties    | String        |
+---------------+---------------+---------------+---------------+
| Jd            | jobDescriptio | properties    | String        |
|               | n             |               |               |
+---------------+---------------+---------------+---------------+
| jsm           | jobStepManage | properties    | String        |
|               | r             |               |               |
+---------------+---------------+---------------+---------------+
| K             | jobWaitDone   | notify        | String        |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| k             | checkPoint    | properties    | String        |
+---------------+---------------+---------------+---------------+
| L             | loginShell    | properties    | String        |
+---------------+---------------+---------------+---------------+
| Lp            | licenseProjec | schedule      | String        |
|               | tName         |               |               |
+---------------+---------------+---------------+---------------+
| ln_mem        | launchNodeMem | resource      | Numeric       |
+---------------+---------------+---------------+---------------+
| ln_slots      | launchNodeSlo | resource      | Numeric       |
|               | ts            |               |               |
+---------------+---------------+---------------+---------------+
| M             | memLimit      | limit         | String        |
+---------------+---------------+---------------+---------------+
| m             | machines      | resource      | String        |
+---------------+---------------+---------------+---------------+
| mig           | migThreshold  | schedule      | Numeric       |
+---------------+---------------+---------------+---------------+
| N             | notifyJobDone | notify        | String        |
+---------------+---------------+---------------+---------------+
| Ne            | notifyJobExit | notify        | String        |
+---------------+---------------+---------------+---------------+
| n             | numTasks      | resource      | String        |
+---------------+---------------+---------------+---------------+
| notify        | notifyJobAny  | notify        | String        |
+---------------+---------------+---------------+---------------+
| network       | networkReq    | resource      | String        |
+---------------+---------------+---------------+---------------+
| nnodes        | numNodes      | resource      | Numeric       |
+---------------+---------------+---------------+---------------+
| o             | outputAppendF | io            | String        |
|               | ile           |               |               |
+---------------+---------------+---------------+---------------+
| oo            | outputOverwri | io            | String        |
|               | teFile        |               |               |
+---------------+---------------+---------------+---------------+
| outdir        | outputDir     | io            | String        |
+---------------+---------------+---------------+---------------+
| P             | projectName   | properties    | String        |
+---------------+---------------+---------------+---------------+
| p             | processLimit  | limit         | String        |
+---------------+---------------+---------------+---------------+
| pack          | pack          | pack          | String        |
+---------------+---------------+---------------+---------------+
| ptl           | pendTimeLimit | limit         | String        |
+---------------+---------------+---------------+---------------+
| Q             | exitCode      | properties    | String        |
+---------------+---------------+---------------+---------------+
| q             | queueName     | properties    | String        |
+---------------+---------------+---------------+---------------+
| R             | resReq        | resource      | String array  |
+---------------+---------------+---------------+---------------+
| r             | rerun         | properties    | String        |
+---------------+---------------+---------------+---------------+
| rn            | rerunNever    | properties    | String        |
+---------------+---------------+---------------+---------------+
| rnc           | resizeNotifCm | properties    | String        |
|               | d             |               |               |
+---------------+---------------+---------------+---------------+
| S             | stackLimit    | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| s             | signal        | properties    | Numeric       |
+---------------+---------------+---------------+---------------+
| sla           | serviceClassN | properties    | String        |
|               | ame           |               |               |
+---------------+---------------+---------------+---------------+
| smt           | smt           | properties    | Numeric       |
+---------------+---------------+---------------+---------------+
| sp            | jobPriority   | properties    | Numeric       |
+---------------+---------------+---------------+---------------+
| stage         | stage         | properties    | String        |
+---------------+---------------+---------------+---------------+
| step_cgroup   | stepCgroup    | properties    | String        |
+---------------+---------------+---------------+---------------+
| T             | threadLimit   | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| t             | specifiedTerm | schedule      | String        |
|               | inateTime     |               |               |
+---------------+---------------+---------------+---------------+
| ti            | terminateInde | schedule      | String        |
|               | pend          |               |               |
+---------------+---------------+---------------+---------------+
| tty           | tty           | io            | String        |
+---------------+---------------+---------------+---------------+
| U             | rsvId         | schedule      | String        |
|               |               | resource      |               |
+---------------+---------------+---------------+---------------+
| u             | mailUser      | notify        | String        |
+---------------+---------------+---------------+---------------+
| ul            | userLimit     | limit         | String        |
+---------------+---------------+---------------+---------------+
| v             | swapLimit     | limit         | String        |
+---------------+---------------+---------------+---------------+
| W             | runtimeLimit  | limit         | String        |
+---------------+---------------+---------------+---------------+
| We            | estimatedRunT | limit         | String        |
|               | ime           |               |               |
+---------------+---------------+---------------+---------------+
| w             | dependency    | schedule      | String        |
+---------------+---------------+---------------+---------------+
| wa            | warningAction | properties    | String        |
+---------------+---------------+---------------+---------------+
| wt            | warningTime   | properties    | String        |
+---------------+---------------+---------------+---------------+
| XF            | x11Forward    | properties    | String        |
+---------------+---------------+---------------+---------------+
| x             | exclusive     | schedule      | String        |
+---------------+---------------+---------------+---------------+
| Zs            | jobSpool      | properties    | String        |
|               |               | script        |               |
+---------------+---------------+---------------+---------------+
| h             | help          |               | String        |
+---------------+---------------+---------------+---------------+
| V             | version       |               | String        |
+---------------+---------------+---------------+---------------+
| command       | cmd           |               | String        |
| The job       |               |               |               |
| command with  |               |               |               |
| arguments or  |               |               |               |
| job script.   |               |               |               |
+---------------+---------------+---------------+---------------+

For more information on the syntax of the key values to specify
for each option, refer to the description of each bsub option in
the IBM Spectrum LSF Command Reference.


# Example



For the following job submission command:

bsub -r -H -N -Ne -i /tmp/input/jobfile.sh -outdir /tmp/output -C 5 -c 2022:12:12 -cn_mem 256 -hostfile /tmp/myHostFile.txt -q normal -G myUserGroup -u "user@example.com" myjob

The following JSON file specifies the equivalent job submission
command:

{  
    "io": {  
        "inputFile": "/tmp/input/jobfile.sh",  
        "outputDir": "/tmp/output"  
    },  
    "limit": {  
        "coreLimit": 5,  
        "cpuTimeLimit": "2022:12:12"  
    },  
    "resource": {  
        "computeNodeMem": 256,  
        "hostFile": "/tmp/myHostFile.txt"  
    },  
    "properties": {  
        "queueName": "normal",  
        "rerun": null  
    },  
    "schedule": {  
        "hold": "",  
        "userGroup": "myUserGroup"  
    },  
    "notify": {  
        "notifyJobDone": "",  
        "notifyJobExit": "",  
        "mailUser": "user@example.com"  
    },  
    "command": "myjob"  
}

Parent topic: Options






**-K**



Submits a job and waits for the job to complete. Sends job status
messages to the terminal.




# Categories



notify, properties



# Synopsis



**bsub -K**


# Conflicting Options



Do not use with the following options: -I, -Ip, -IS, -ISp, -ISs,
-Is, -IX.


# Description



Sends the message "Waiting for dispatch" to the terminal
when you submit the job. Sends the message "Job is
finished" to the terminal when the job is done (if
**LSB\_SUBK\_SHOW\_JOBID** is enabled in the lsf.conf file or as
an environment variable, also displays the job ID when the job is
done).

If **LSB\_SUBK\_SHOW\_EXEC\_HOST** is enabled in lsf.conf, also
sends the message "Starting on _execution\_host_" when
the job starts running on the execution host.

You are not able to submit another job until the job is
completed. This is useful when completion of the job is required
to proceed, such as a job script. If the job needs to be rerun
due to transient failures, bsub returns after the job finishes
successfully. bsub exits with the same exit code as the job so
that job scripts can take appropriate actions based on the exit
codes. bsub exits with value 126 if the job was terminated while
pending.

Parent topic: Options






**-k**



Makes a job checkpointable and specifies the checkpoint
directory.




# Categories



properties



# Synopsis



**bsub -k "**checkpoint_dir
[**init=**initial_checkpoint_period] [checkpoint_period]
[**method=**method\_name]**"**


# Description



Specify a relative or absolute path name. The quotes (") are
required if you specify a checkpoint period, initial checkpoint
period, or custom checkpoint and restart method name.

The job ID and job file name are concatenated to the checkpoint
directory when creating a checkpoint file.

**Note: **The file path of the checkpoint directory can contain
up to 4000 characters for UNIX and Linux, or up to 255 characters
for Windows, including the directory and file name.

When a job is checkpointed, the checkpoint information is stored
in _checkpoint\_dir_/_job\_ID_/_file\_name_. Multiple
jobs can checkpoint into the same directory. The system can
create multiple files.

The checkpoint directory is used for restarting the job (see
brestart(1)). The checkpoint directory can be any valid path.

Optionally, specifies a checkpoint period in minutes. Specify a
positive integer. The running job is checkpointed automatically
every checkpoint period. The checkpoint period can be changed
using bchkpnt. Because checkpointing is a heavyweight operation,
you should choose a checkpoint period greater than half an hour.

Optionally, specifies an initial checkpoint period in minutes.
Specify a positive integer. The first checkpoint does not happen
until the initial period has elapsed. After the first checkpoint,
the job checkpoint frequency is controlled by the normal job
checkpoint interval.

The echkpnt.method\_name and erestart._method\_name_
programs must be in LSF_SERVERDIR or in the directory specified
by **LSB\_ECHKPNT\_METHOD\_DIR** (environment variable or set in
lsf.conf).

If a custom checkpoint and restart method is already specified
with **LSB\_ECHKPNT\_METHOD** (environment variable or in
lsf.conf), the method you specify with bsub -k overrides this.

Process checkpointing is not available on all host types, and may
require linking programs with a special libraries (see
libckpt.a(3)). LSF invokes echkpnt (see echkpnt(8)) found in
**LSF\_SERVERDIR** to checkpoint the job. You can override the
default echkpnt for the job by defining as environment variables
or in lsf.conf **LSB\_ECHKPNT\_METHOD** and
**LSB\_ECHKPNT\_METHOD\_DIR** to point to your own echkpnt. This
allows you to use other checkpointing facilities, including
application-level checkpointing.

The checkpoint method directory should be accessible by all users
who need to run the custom echkpnt and erestart programs.

Only running members of a chunk job can be checkpointed.

Parent topic: Options






**-L**



Initializes the execution environment using the specified login
shell.




# Categories



properties



# Synopsis



**bsub -L **login_shell


# Description



The specified login shell must be an absolute path. This is not
necessarily the shell under which the job is executed.

Login shell is not supported on Windows.

On UNIX and Linux, the file path of the login shell can contain
up to 58 characters.

If -L conflicts with -env, the value of -L takes effect.

Parent topic: Options






**-ln\_mem**



Specifies the memory limit on the launch node (LN) host for the
CSM job.




# Categories



resource



# Synopsis



**bsub -ln\_mem** mem_size


# Conflicting Options



Do not use with the -csm y option.


# Description



This option is for easy mode LSF job submission to run with the
IBM Cluster Systems Manager (CSM) integration package.

Parent topic: Options






**-ln\_slots**



Specifies the number of slots to use on the launch node (LN) host
for the CSM job.




# Categories



resource



# Synopsis



**bsub -ln\_slots** num_slots


# Conflicting Options



Do not use with the -csm y option.


# Description



This option is for easy mode LSF job submission to run with the
IBM Cluster Systems Manager (CSM) integration package.

Parent topic: Options






**-Lp**



Assigns the job to the specified LSF License Scheduler project.




# Categories



schedule



# Synopsis



**bsub -Lp **ls_project_name

Parent topic: Options






**-M**



Sets a memory limit for all the processes that belong to the job.




# Categories



limit



# Synopsis



**-M **mem_limit [**!**]


# Description



By default, LSF sets a per-process (soft) memory limit for all
the processes that belong to the job. To set a hard memory limit,
specify an exclamation point (!) after the memory limit. If
the job reaches the hard memory limit, LSF kills the job as soon
as it exceeds the memory limit without waiting for the host
memory and swap threshold to be reached.

By default, the limit is specified in KB. Use the
**LSF\_UNIT\_FOR\_LIMITS** parameter in the lsf.conf file to
specify a larger unit for the limit.

You can use the following units for limits:

*  KB or K (kilobytes)

*  MB or M (megabytes)

*  GB or G (gigabytes)

*  TB or T (terabytes)

*  PB or P (petabytes)

*  EB or E (exabytes)

*  ZB or Z (zettabytes)

If the **LSB\_MEMLIMIT\_ENFORCE** or **LSB\_JOB\_MEMLIMIT**
parameters are set to y in the lsf.conf file, or if you set
the hard memory limits with an exclamation point (!), LSF
kills the job when it exceeds the memory limit or if the host
memory and swap threshold is reached. Otherwise, LSF passes the
memory limit to the operating system. UNIX operating systems that
support RUSAGE\_RSS for the setrlimit() function can apply
the memory limit to each process.

The Windows operating system does not support the memory limit at
the OS level.

Parent topic: Options






**-m**



Submits a job to be run on specific hosts, host groups, or
compute units.




# Categories



resource



# Synopsis



**bsub -m "**host\_name[[**!**] | **+**[pref_level]] |
host\_group[[**!**] | **+**[pref_level |
compute\_unit[[**!**] | **+**[pref_level]] ...**"**

**bsub -m "**host\_name**@**cluster_name[ |
**+**[pref_level]] | host\_group**@**cluster_name[ |
**+**[pref_level | compute\_unit**@**cluster_name[ |
**+**[pref_level]] ...**"**


# Description



By default, if multiple hosts are candidates, runs the job on the
least-loaded host.

When a compute unit requirement is specified along with a host,
host group, or compute unit preference, these preferences will
affect the order of a compute unit. The highest preference of
hosts within the compute unit is taken as the preference of this
compute unit. Thus, the compute unit containing the highest
preference host will be considered first. In addition, the job
will be rejected unless:

*  A host in the list belongs to a compute unit, and

*  A host in the first execution list belongs to a compute unit.

When used with a compound resource requirement, the first host
allocated must satisfy the simple resource requirement string
appearing first in the compound resource requirement.

To change the order of preference, put a plus (+) after the names
of hosts or host groups that you would prefer to use, optionally
followed by a preference level. For preference level, specify a
positive integer, with higher numbers indicating greater
preferences for those hosts. For example, -m "hostA groupB+2
hostC+1" indicates that groupB is the most preferred and
hostA is the least preferred.

The keyword others can be specified with or without a
preference level to refer to other hosts not otherwise listed.
The keyword others must be specified with at least one host
name or host group, it cannot be specified by itself. For
example, -m "hostA+ others" means that hostA is preferred
over all other hosts.

If you also use -q, the specified queue must be configured to
include at least a partial list of the hosts in your host list.
Otherwise, the job is not submitted. To find out what hosts are
configured for the queue, use bqueues -l.

If the host group contains the keyword all, LSF dispatches
the job to any available host, even if the host is not defined
for the specified queue.

To display configured host groups and compute units, use bmgroup.

For the job forwarding model with the LSF multicluster
capability, specify a remote host using
_host\_name_@_cluster\_name_ and change the order of
preference by using the plus (+) sign.

For parallel jobs, specify first execution host candidates when
you want to ensure that a host has the required resources or
runtime environment to handle processes that run on the first
execution host.

To specify one or more hosts or host groups as first execution
host candidates, add an exclamation point (!) after the
host name, as shown in the following example:

bsub -n 2 -m "host1 host2! hostgroupA! host3 host4"
my\_parallel\_job

In this example, LSF runs my\_parallel\_job according to the
following steps:

1. LSF selects either host2 or a host defined in hostgroupA
   as the first execution host for the parallel job.

   **Note: **First execution host candidates specified at the
   job-level (command line) override candidates defined at the
   queue level (in lsb.queues).

2. If any of the first execution host candidates have enough
   processors to run the job, the entire job runs on the first
   execution host, and not on any other hosts.

   In the example, if host2 or a member of hostgroupA
   has two or more processors, the entire job runs on the first
   execution host.

3. If the first execution host does not have enough processors to
   run the entire job, LSF selects additional hosts that are not
   defined as first execution host candidates.

   Follow these guidelines when you specify first execution host
   candidates:

   *  If you specify a host group, you must first define the host
      group in the file lsb.hosts.

   *  Do not specify a dynamic host group as a first execution
      host.

   *  Do not specify all, allremote, or others, or a host
      partition as a first execution host.

   *  Do not specify a preference (+) for a host identified
      as a first execution host candidate (using !).

   *  For each parallel job, specify enough regular hosts to
      satisfy the processor requirement for the job. Once LSF
      selects a first execution host for the current job, the
      other first execution host candidates become unavailable to
      the current job, but remain available to other jobs as
      either regular or first execution hosts.

   *  You cannot specify first execution host candidates when you
      use the brun command.

When using the LSF multicluster capability, insert an exclamation
point (!) after the cluster name, as shown in the following
example:

bsub -n 2 -m "host2@cluster2! host3@cluster2"
my\_parallel\_job

When specifying compute units, the job runs within the listed
compute units. Used in conjunction with a mandatory first
execution host, the compute unit containing the first execution
host is given preference.

In the following example one host from host group hg
appears first, followed by other hosts within the same compute
unit. Remaining hosts from other compute units appear grouped by
compute, and in the same order as configured in the ComputeUnit
section of lsb.hosts.

bsub -n 64 -m "hg! cu1 cu2 cu3 cu4" -R "cu[pref=config]"
my\_job


# Examples



bsub -m "host1 host3 host8 host9" my\_program

Submits my\_program to run on one of the candidate hosts:
host1, host3, host8, or host9.

Parent topic: Options






**-mig**



Specifies the migration threshold for checkpointable or
re-runnable jobs, in minutes.




# Categories



schedule



# Synopsis



**bsub -mig** migration_threshold


# Description



Enables automatic job migration and specifies the migration
threshold, in minutes. A value of 0 (zero) specifies that a
suspended job should be migrated immediately.

Command-level job migration threshold overrides application
profile and queue-level settings.

Where a host migration threshold is also specified, and is lower
than the job value, the host value is used.

Parent topic: Options






**-N**



Sends the job report to you by mail when the job finishes.




# Categories



notify



# Synopsis



**bsub -N**


# Description



When used without any other options, behaves the same as the
default.

Use only with -o, -oo, -I, -Ip, and -Is options, which do not
send mail, to force LSF to send you a mail message when the job
is done.


# Conflicting Options



Do not use with the bsub -Ne option. The bmod -Nn command option
cancels both the -N and -Ne mail notification options.

Parent topic: Options






**-n**



Submits a parallel job and specifies the number of tasks in the
job.




# Categories



resource



# Synopsis



**bsub -n** min\_tasks[**,**max_tasks]


# Description



The number of tasks is used to allocate a number of slots for the
job. Usually, the number of slots assigned to a job will equal
the number of tasks specified. For example, one task will be
allocated with one slot. (Some slots/processors may be on the
same multiprocessor host).

You can specify a minimum and maximum number of tasks. For
example, this job requests a minimum of 4, but can launch up to 6
tasks:

bsub -n 4,6 a.out

The job can start if at least the minimum number of
slots/processors is available for the minimum number of tasks
specified. If you do not specify a maximum number of tasks, the
number you specify represents the exact number of tasks to
launch.

If **PARALLEL\_SCHED\_BY\_SLOT=Y** in lsb.params, this option
specifies the number of slots required to run the job, not the
number of processors.

When used with the -R option and a compound resource requirement,
the number of slots in the compound resource requirement must be
compatible with the minimum and maximum tasks specified.

Jobs that have fewer tasks than the minimum **TASKLIMIT**
defined for the queue or application profile to which the job is
submitted, or more tasks than the maximum **TASKLIMIT** are
rejected. If the job has minimum and maximum tasks, the maximum
tasks requested cannot be less than the minimum **TASKLIMIT**,
and the minimum tasks requested cannot be more than the maximum
**TASKLIMIT**.

For example, if the queue defines TASKLIMIT=4 8:

*  bsub -n 6 is accepted because it requests slots within the
   range of **TASKLIMIT**

*  bsub -n 9 is rejected because it requests more tasks than the
   **TASKLIMIT** allows

*  bsub -n 1 is rejected because it requests fewer tasks than the
   **TASKLIMIT** allows

*  bsub -n 6,10 is accepted because the minimum value 6 is within
   the range of the **TASKLIMIT** setting

*  bsub -n 1,6 is accepted because the maximum value 6 is within
   the range of the **TASKLIMIT** setting

*  bsub -n 10,16 is rejected because its range is outside the
   range of **TASKLIMIT**

*  bsub -n 1,3 is rejected because its range is outside the range
   of **TASKLIMIT**

See the **TASKLIMIT** parameter in lsb.queues and
lsb.applications for more information.

If **JOB\_SIZE\_LIST** is defined in lsb.applications or
lsb.queues and a job is submitted to a queue or an application
profile with a job size list, the requested job size in the job
submission must request only a single job size (number of tasks)
rather than a minimum and maximum value and the requested job
size in the job submission must satisfy the list defined in
**JOB\_SIZE\_LIST**, otherwise LSF rejects the job submission. If
a job submission does not include a job size request, LSF assigns
the default job size to the submission request.
**JOB\_SIZE\_LIST** overrides any **TASKLIMIT** parameters
defined at the same level.

For example, if the application profile or queue defines
JOB_SIZE_LIST=4 2 10 6 8:

*  bsub -n 6 is accepted because it requests a job size that is
   in **JOB\_SIZE\_LIST**.

*  bsub -n 9 is rejected because it requests a job size that is
   not in **JOB\_SIZE\_LIST**.

*  bsub -n 2,8 is rejected because you cannot request a range of
   job slot sizes when **JOB\_SIZE\_LIST** is defined.

*  bsub without specifying a job size (using -n or -R) is
   accepted and the job submission is assigned a job size request
   of 4 (the default value).

See the **JOB\_SIZE\_LIST** parameter in lsb.applications(5) or
lsb.queues(5) for more information.

In the LSF multicluster capability environment, if a queue
exports jobs to remote clusters (see the **SNDJOBS\_TO**
parameter in lsb.queues), then the process limit is not imposed
on jobs submitted to this queue.

Once the required number of processors is available, the job is
dispatched to the first host selected. The list of selected host
names for the job are specified in the environment variables
**LSB\_HOSTS** and **LSB\_MCPU\_HOSTS**. The job itself is
expected to start parallel components on these hosts and
establish communication among them, optionally using RES.

Specify first execution host candidates using the -m option when
you want to ensure that a host has the required resources or
runtime environment to handle processes that run on the first
execution host.

If you specify one or more first execution host candidates, LSF
looks for a first execution host that satisfies the resource
requirements. If the first execution host does not have enough
processors or job slots to run the entire job, LSF looks for
additional hosts.


# Examples



bsub –n2 –R "span[ptile=1]" –network "protocol=mpi,lapi:
type=sn_all: instances=2: usage=shared" poe
/home/user1/mpi\_prog

For this job running on hostA and hostB, each task
will reserve 8 windows (2*2*2), for 2 protocols, 2 instances and
2 networks. If enough network windows are available, other
network jobs with usage=shared can run on hostA and
hostB because networks used by this job are shared.

Parent topic: Options






**-Ne**



Sends the job report to you by mail when the job exits (that is,
when the job is in Exit status).




# Categories



notify



# Synopsis



**bsub -Ne**


# Description



This option ensures that an email notification is only sent on a
job error.

When used without any other options, behaves the same as the
default.

Use only with -o, -oo, -I, -Ip, and -Is options, which do not
send mail, to force LSF to send you a mail message when the job
exits.


# Conflicting Options



Do not use with the bsub -N option. The bmod -Nn command option
cancels both the -N and -Ne mail notification options.

Parent topic: Options






**-network**



For LSF IBM Parallel Environment (IBM PE) integration. Specifies
the network resource requirements to enable network-aware
scheduling for IBM PE jobs.




# Categories



resource



# Synopsis



**bsub -network "** network\_res\_req**"**


# Description



**Note: **This command option is deprecated and might be
removed in a future version of LSF.

If any network resource requirement is specified in the job,
queue, or application profile, the jobs are treated as IBM PE
jobs. IBM PE jobs can only run on hosts where IBM PE pnsd daemon
is running.

The network resource requirement string _network\_res\_req_ has
the same syntax as the NETWORK_REQ parameter defined in
lsb.applications or lsb.queues.

_network\_res\_req_ has the following syntax:

[type=sn_all | sn_single]
[:protocol=_protocol\_name_[(_protocol\_number_)][,_protocol\_name_[(_protocol\_number_)]]
[:mode=US | IP] [:usage=shared | dedicated]
[:instance=_positive\_integer_]

**LSF\_PE\_NETWORK\_NUM** must be defined to a non-zero value in
lsf.conf for the LSF to recognize the -network option. If
**LSF\_PE\_NETWORK\_NUM** is not defined or is set to 0, the job
submission is rejected with a warning message.

The -network option overrides the value of NETWORK_REQ defined in
lsb.applications or lsb.queues.

The following network resource requirement options are supported:

**type=sn_all | sn\_single**  
         Specifies the adapter device type to use for message
         passing: either sn_all or sn_single.

         **sn\_single**  
                  When used for switch adapters, specifies that
                  all windows are on a single network

         **sn\_all**  
                  Specifies that one or more windows are on each
                  network, and that striped communication should
                  be used over all available switch networks. The
                  networks specified must be accessible by all
                  hosts selected to run the IBM PE job. See the
                  Parallel Environment Runtime Edition for AIX:
                  Operation and Use guide (SC23-6781-05) for more
                  information about submitting jobs that use
                  striping.

         If mode is IP and type is specified as sn_all or
         sn_single, the job will only run on IB adapters (IPoIB).
         If mode is IP and type is not specified, the job will
         only run on Ethernet adapters (IPoEth). For IPoEth jobs,
         LSF ensures the job is running on hosts where pnsd is
         installed and running. For IPoIB jobs, LSF ensures the
         job the job is running on hosts where pnsd is installed
         and running, and that InfiniBand networks are up.
         Because IP jobs do not consume network windows, LSF does
         not check if all network windows are used up or the
         network is already occupied by a dedicated IBM PE job.

         Equivalent to the IBM PE MP_EUIDEVICE environment
         variable and -euidevice IBM PE flag See the Parallel
         Environment Runtime Edition for AIX: Operation and Use
         guide (SC23-6781-05) for more information. Only sn_all
         or sn_single are supported by LSF. The other types
         supported by IBM PE are not supported for LSF jobs.

**protocol=protocol\_name[(protocol\_number)]**  
         Network communication protocol for the IBM PE job,
         indicating which message passing API is being used by
         the application. The following protocols are supported
         by LSF:

         **mpi**  
                  The application makes only MPI calls. This
                  value applies to any MPI job regardless of the
                  library that it was compiled with (IBM PE MPI,
                  MPICH2).

         **pami**  
                  The application makes only PAMI calls.

         **lapi**  
                  The application makes only LAPI calls.

         **shmem**  
                  The application makes only OpenSHMEM calls.

         **user\_defined\_parallel\_api**  
                  The application makes only calls from a
                  parallel API that you define. For example:
                  protocol=myAPI or protocol=charm.

         The default value is mpi.

         LSF also supports an optional _protocol\_number_ (for
         example, mpi(2), which specifies the number of
         contexts (endpoints) per parallel API instance. The
         number must be a power of 2, but no greater than 128 (1,
         2, 4, 8, 16, 32, 64, 128). LSF will pass the
         communication protocols to IBM PE without any change.
         LSF will reserve network windows for each protocol.

         When you specify multiple parallel API protocols, you
         cannot make calls to both LAPI and PAMI (lapi, pami) or
         LAPI and OpenSHMEM (lapi, shmem) in the same
         application. Protocols can be specified in any order.

         See the MP_MSG_API and MP_ENDPOINTS environment
         variables and the -msg_api and -endpoints IBM PE flags
         in the Parallel Environment Runtime Edition for AIX:
         Operation and Use guide (SC23-6781-05) for more
         information about the communication protocols that are
         supported by IBM PE.

**mode=US | IP**  
         The network communication system mode used by the
         communication specified communication protocol: US (User
         Space) or IP (Internet Protocol). The default value is
         US. A US job can only run with adapters that support
         user space communications, such as the IB adapter. IP
         jobs can run with either Ethernet adapters or IB
         adapters. When IP mode is specified, the instance number
         cannot be specified, and network usage must be
         unspecified or shared.

         Each instance on the US mode requested by a task running
         on switch adapters requires and adapter window. For
         example, if a task requests both the MPI and LAPI
         protocols such that both protocol instances require US
         mode, two adapter windows will be used.

**usage=dedicated | shared**  
         Specifies whether the adapter can be shared with tasks
         of other job steps: dedicated or shared. Multiple tasks
         of the same job can share one network even if usage is
         dedicated.

**instance=positive\_integer**  
         The number of parallel communication paths (windows) per
         task made available to the protocol on each network. The
         number actually used depends on the implementation of
         the protocol subsystem.

         The default value is 1.

         If the specified value is greater than
         MAX_PROTOCOL_INSTANCES in lsb.params or lsb.queues, LSF
         rejects the job.

The following IBM LoadLeveller job command file options are not
supported in LSF:

*  collective_groups

*  imm_send_buffers

*  rcxtblocks

See Administering IBM Spectrum LSF for more information about
network-aware scheduling and running and managing workload
through IBM Parallel Environment.


# Examples



bsub –n2 –R "span[ptile=1]" –network "protocol=mpi,lapi:
type=sn_all: instances=2: usage=shared" poe
/home/user1/mpi\_prog

For this job running on hostA and hostB, each task
will reserve 8 windows (2*2*2), for 2 protocols, 2 instances and
2 networks. If enough network windows are available, other
network jobs with usage=shared can run on hostA and
hostB because networks used by this job are shared.

Parent topic: Options






**-nnodes**



Specifies the number of compute nodes that are required for the
CSM job.




# Categories



resource



# Synopsis



**bsub -nnodes** num_nodes


# Conflicting Options



Do not use with the -csm y option.


# Description



This option is for easy mode LSF job submission to run with the
IBM Cluster Systems Manager (CSM) integration package.


# Default



1

Parent topic: Options






**-notify**



Requests that the user be notified when the job reaches any of
the specified states.




# Categories



notify



# Synopsis



**bsub -notify "**[**exit **] [**done **] [**start **]
[**suspend**]**"**


# Description



Use a space to separate multiple job states. The notification
request is logged in **JOB\_NEW** events.

Parent topic: Options






**-o**



Appends the standard output of the job to the specified file
path.




# Categories



io



# Synopsis



**bsub -o **output_file


# Description



Sends the output by mail if the file does not exist, or the
system has trouble writing to it.

If only a file name is specified, LSF writes the output file to
the current working directory. If the current working directory
is not accessible on the execution host after the job starts, LSF
writes the standard output file to /tmp/.

If the specified _output\_file_ path is not accessible, the
output will not be stored.

If you use the special character %J in the name of the
output file, then %J is replaced by the job ID of the job.
If you use the special character %I in the name of the
output file, then %I is replaced by the index of the job in
the array, if the job is a member of an array. Otherwise,
%I is replaced by 0 (zero).

**Note: **The file path can contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows, including
the directory, file name, and expanded values for %J
(_job\_ID_) and %I (_index\_ID_).

If the parameter **LSB\_STDOUT\_DIRECT** in lsf.conf is set to Y
or y, the standard output of a job is written to the file you
specify as the job runs. If **LSB\_STDOUT\_DIRECT** is not set,
it is written to a temporary file and copied to the specified
file after the job finishes. **LSB\_STDOUT\_DIRECT** is not
supported on Windows.

If you use -o without -e or -eo, the standard error of the job is
stored in the output file.

If you use -o with -e or -eo and the specified _error\_file_
is the same as the _output\_file_, the _error\_file_ of the
job is NULL and the standard error of the job is stored in the
output file.

If you use -o without -N, the job report is stored in the output
file as the file header.

If you use both -o and -N, the output is stored in the output
file and the job report is sent by mail. The job report itself
does not contain the output, but the report advises you where to
find your output.


# Examples



bsub -q short -o my_output_file "pwd; ls" 

Submit the UNIX command pwd and ls as a job to the queue named
short and store the job output in my_output file.

Parent topic: Options






**-oo**



Overwrites the standard output of the job to the specified file
path.




# Categories



io



# Synopsis



**bsub -oo **output_file


# Description



Overwrites the standard output of the job to the specified file
if it exists, or sends the output to a new file if it does not
exist. Sends the output by mail if the system has trouble writing
to the file.

If only a file name is specified, LSF writes the output file to
the current working directory. If the current working directory
is not accessible on the execution host after the job starts, LSF
writes the standard output file to /tmp/.

If the specified _output\_file_ path is not accessible, the
output will not be stored.

If you use the special character %J in the name of the
output file, then %J is replaced by the job ID of the job.
If you use the special character %I in the name of the
output file, then %I is replaced by the index of the job in
the array, if the job is a member of an array. Otherwise,
%I is replaced by 0 (zero).

**Note: **The file path can contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows, including
the directory, file name, and expanded values for %J
(_job\_ID_) and %I (_index\_ID_).

If the parameter **LSB\_STDOUT\_DIRECT** in lsf.conf is set to Y
or y, the standard output of a job overwrites the output file you
specify as the job runs, which occurs every time the job is
submitted with the overwrite option, even if it is requeued
manually or by the system. If **LSB\_STDOUT\_DIRECT** is not set,
the output is written to a temporary file that overwrites the
specified file after the job finishes. **LSB\_STDOUT\_DIRECT** is
not supported on Windows.

If you use -oo without -e or -eo, the standard error of the job
is stored in the output file.

If you use -oo without -N, the job report is stored in the output
file as the file header.

If you use both -oo and -N, the output is stored in the output
file and the job report is sent by mail. The job report itself
does not contain the output, but the report advises you where to
find your output.

Parent topic: Options






**-outdir**



Creates the job output directory.




# Categories



io



# Synopsis



**bsub -outdir **output_directory


# Description



The path for the output directory can include the following
dynamic patterns, which are case sensitive:

*  %J - job ID

*  %JG - job group (if not specified, it will be ignored)

*  %I - index (default value is 0)

*  %EJ - execution job ID

*  %EI - execution index

*  %P - project name

*  %U - user name

*  %G - User group new forthe job output directory

*  %H - first execution host name

For example, the system creates the submission_dir/user1/jobid_0/
output directory for the job with the following command:

bsub -outdir "%U/%J_%I" myprog

If the cluster wide output directory was defined but the outdir
option was not set, for example,
**DEFAULT\_JOB\_OUTDIR**=/scratch/joboutdir/%U/%J_%I in
lsb.params, the system creates the
/scratch/joboutdir/user1/jobid_0/ output directory for the job
with the following command:

bsub myprog

If the submission directory is /scratch/joboutdir/ on the shared
file system and you want the system to create
/scratch/joboutdir/user1/jobid_0/ for the job output directory,
then run the following command:

bsub -outdir "%U/%J_%I" myjob

Since the command path can contain up to 4094 characters for UNIX
and Linux, or up to 255 characters for Windows, including the
directory and file name, the outdir option does not have its own
length limitation. The outdir option supports mixed UNIX and
Windows paths when **LSB\_MIXED\_PATH\_ENABLE=Y/y**.
**LSB\_MIXED\_PATH\_DELIMITER** controls the delimiter.

The following assumptions and dependencies apply to the -outdir
command option:

*  The execution host has access to the submission host.

*  The submission host should be running RES or it will use
   EGO_RSH to run a directory creation command. If this parameter
   is not defined, rsh will be used. RES should be running on the
   Windows submission host in order to create the output
   directory.

Parent topic: Options






**-P**



Assigns the job to the specified project.




# Categories



properties



# Synopsis



**bsub -P **project_name


# Description



The project does not have to exist before submitting the job.

Project names can be up to 511 characters long.

Parent topic: Options






**-p**



Sets the limit of the number of processes to the specified value
for the whole job.




# Categories



limit



# Synopsis



**bsub -p **process_limit


# Description



The default is no limit. Exceeding the limit causes the job to
terminate.

Parent topic: Options






**-pack**



Submits job packs instead of an individual job.




# Categories



pack



# Synopsis



**bsub -pack **job_submission_file


# Conflicting Options



Do not use with any other bsub option in the command line.


# Description



The purpose of the job packs feature is to speed up the
submission of a large number of jobs. When job pack submission is
enabled, you can submit jobs by submitting a single file
containing multiple job requests.

Specify the full path to the job submission file. The job packs
feature must be enabled (by defining **LSB\_MAX\_PACK\_JOBS** in
lsb.conf) to use the -pack option.

In the command line, this option is not compatible with any other
bsub options. Do not put any other bsub options in the command
line, as they must be included in each individual job request in
the file.

In the job submission file, define one job request per line,
using normal bsub syntax but omitting the word "bsub". For
requests in the file, job pack submission supports all bsub
options in the job submission file except for the following:

-I, -Ip, -Is, -IS, -ISp, -ISs, -IX, -XF, -K, -jsdl, -h, -V,
-pack.

When you use the job packs feature to submit multiple jobs to
mbatchd at once, instead of submitting the jobs individually, it
minimizes system overhead and improves the overall job submission
rate dramatically. When you use this feature, you create a job
submission file that defines each job request. You specify all
the bsub options individually for each job, so unlike chunk jobs
and job arrays, there is no need for jobs in this file to have
anything in common. To submit the jobs to LSF, you simply submit
the file using the bsub -pack option.

LSF parses the file contents and submits the job requests to
mbatchd, sending multiple requests at one time. Each group of
jobs submitted to mbatchd together is called a job pack. The job
submission file can contain any number of job requests, and LSF
will group them into job packs automatically. The reason to group
jobs into packs is to maintain proper mbatchd performance: while
mbatchd is processing a job pack, mbatchd is blocked from
processing other requests, so limiting the number of jobs in each
pack ensures a reasonable mbatchd response time for other job
submissions. Job pack size is configurable.

If the cluster configuration is not consistent and mbatchd
receives a job pack that exceeds the job pack size defined in
lsf.conf, it will be rejected.

Once the pack is submitted to mbatchd, each job request in the
pack is handled by LSF as if it was submitted individually with
the bsub command.

Parent topic: Options



**Enabling job packs**



Job packs are disabled by default. Before running bsub -pack, you
must enable the job packs feature.

1. Edit lsf.conf.

2. Define the parameter LSB\_MAX\_PACK\_JOBS=100.

   Defining this parameter enables the job packs feature and sets
   the job pack size. Set 100 as the initial pack size and modify
   the value as needed. If set to 1, jobs from the file are
   submitted individually, as if submitted directly using the
   bsub command. If set to 0, job packs are disabled.



3. Optionally, define the parameter LSB\_PACK\_MESUB=N.

   Do this if you want to further increase the job submission
   rate by preventing the execution of any mesub during job
   submission. This parameter only affects the jobs submitted
   using job packs.



4. Optionally, define the parameter LSB\_PACK\_SKIP\_ERROR=Y.

   Do this if you want LSF to process all requests in a job
   submission file, and continue even if some requests have
   errors.



5. Restart mbatchd to make your changes take effect.



**Submitting job packs**



1. Prepare the job submission file.

   The job submission file is a text file containing all the jobs
   that you want to submit. Each line in the file is one job
   request. For each request, the syntax is identical to the bsub
   command line without the word "bsub".

   For example,

   #This file contains 2 job requests.  
   -R "select[mem&gt;200] rusage[mem=100]" job1.sh  
   -R "select[swap&gt;400] rusage[swap=200]" job2.sh  
   #end



   The job submission file has the following limitations:

   *  The following bsub options are not supported:

      -I, -Ip, -Is, -IS,-ISp, -ISs, -IX, -XF, -K, -jsdl, -h, -V,
      -pack.

   *  Terminal Services jobs are not supported.

   *  I/O redirection is not supported.

   *  Blank lines and comment lines (beginning with #) are
      ignored. Comments at the end of a line are not supported.

   *  Backslash ( is not considered a special character to join
      two lines.

   *  Shell scripting characters are treated as plain text, they
      will not be interpreted.

   *  Matched pairs of single and double quotations are
      supported, but they must have space before and after. For
      example, -J "job1" is supported, -J"job1" is not, and -J
      "job"1 is not.

   For job dependencies, use the job name instead of job ID to
   specify the dependency condition. A job request will be
   rejected if the job name or job ID of the job it depends on
   does not already exist.



2. After preparing the file, use the bsub -pack option to submit
   all the jobs in the file.

   Specify the full path to the job submission file. Do not put
   any other bsub options in the command line, they must be
   included in each individual job request in the file.







**-ptl**



Specifies the pending time limit for the job.




# Categories



limit



# Synopsis



**bsub -ptl** [hour**:**]minute

# Description



LSF sends the job-level pending time limit configuration to IBM
Spectrum LSF RTM (LSF RTM), which handles the alarm and triggered
actions such as user notification (for example, notifying the
user that submitted the job and the LSF administrator) and job
control actions (for example, killing the job). LSF RTM compares
the job's pending time to the pending time limit, and if the job
is pending for longer than this specified time limit, LSF RTM
triggers the alarm and actions. This parameter works without LSF
RTM, but LSF does not take any other alarm actions.

In MultiCluster job forwarding mode, the job's pending time limit
is ignored in the execution cluster, while the submission cluster
merges the job's queue-, application-, and job-level pending time
limit according to local settings.

The pending time limit is in the form of
[_hour_:]_minute_. The minutes can be specified as
a number greater than 59. For example, three and a half hours can
either be specified as 3:30, or 210.

The job-level pending time limit specified here overrides any
application-level or queue-level limits specified
(**PEND\_TIME\_LIMIT** in lsb.applications and lsb.queues).

Parent topic: Options






**-Q**



Specify automatic job re-queue exit values.




# Categories



properties



# Synopsis



**bsub -Q "**exit_code [exit_code ...] [**EXCLUDE(**exit_code
 ...**)**]**"**


# Description



Use spaces to separate multiple exit codes. The reserved keyword
all specifies all exit codes. Exit codes are typically between 0
and 255. Use a tilde (~) to exclude specified number or numbers
from the list.

_exit\_code_ has the following form:

"[all] [~number ...] | [number ...]"  


Job level exit values override application-level and queue-level
values.

Jobs running with the specified exit code share the same
application and queue with other jobs.

Define an exit code as EXCLUDE(_exit\_code_) to enable
exclusive job requeue. Exclusive job requeue does not work for
parallel jobs.

If mbatchd is restarted, it does not remember the previous hosts
from which the job exited with an exclusive requeue exit code. In
this situation, it is possible for a job to be dispatched to
hosts on which the job has previously exited with an exclusive
exit code.

Parent topic: Options






**-q**



Submits the job to one of the specified queues.




# Categories



properties



# Synopsis



**bsub -q "**queue_name ...**"**


# Description



Quotes are optional for a single queue. The specified queues must
be defined for the local cluster. For a list of available queues
in your local cluster, use bqueues.

When a list of queue names is specified, LSF attempts to submit
the job to the first queue listed. If that queue cannot be used
because of the job's resource limits or other restrictions, such
as the requested hosts, your accessibility to a queue, queue
status (closed or open), then the next queue listed is
considered. The order in which the queues are considered is the
same order in which these queues are listed.


# Examples



bsub -q short -o my_output_file "pwd; ls" 

Submit the UNIX command pwd and ls as a job to the queue named
short and store the job output in my_output file.

bsub -q "queue1 queue2 queue3" -c 5 my\_program

Submit my\_program to one of the candidate queues:
queue1, queue2, and queue3 that are selected
according to the CPU time limit specified by -c 5.

Parent topic: Options






**-R**



Runs the job on a host that meets the specified resource
requirements.




# Categories



resource



# Synopsis



**bsub -R "**res\_req**"** [**-R** "res_req" ...]

Parent topic: Options



**Description**



A resource requirement string describes the resources a job
needs. LSF uses resource requirements to select hosts for job
execution. Resource requirement strings can be simple (applying
to the entire job), compound (applying to the specified number of
slots), or alternative.

Simple resource requirement strings are divided into the
following sections. Each section has a different syntax.

*  A selection section (select). The selection section specifies
   the criteria for selecting execution hosts from the system.

*  An ordering section (order). The ordering section indicates
   how the hosts that meet the selection criteria should be
   sorted.

*  A resource usage section (rusage). The resource usage section
   specifies the expected resource consumption of the task.

*  A job spanning section (span). The job spanning section
   indicates if a parallel job should span across multiple hosts.

*  A same resource section (same). The same section indicates
   that all processes of a parallel job must run on the same type
   of host.

*  A compute unit resource section (cu). The compute unit section
   specifies topological requirements for spreading a job over
   the cluster.

*  A CPU and memory affinity resource section (affinity). The
   affinity section specifies CPU and memory binding requirements
   for tasks of a job.

The resource requirement string sections have the following
syntax:

select[selection_string] order[order_string] rusage[  
usage_string [, usage_string][|| usage_string] ...]  
span[span_string] same[same_string] cu[cu_string]] affinity[affinity_string]

The square brackets must be typed as shown for each section. A
blank space must separate each resource requirement section.

For example, to submit a job that runs on Red Hat Enterprise
Linux 6 or Red Hat Enterprise Linux 7:

bsub -R "rhel6 || rhel7" myjob  


The following command runs the job called myjob on an HP-UX
host that is lightly loaded (CPU utilization) and has at least 15
MB of swap memory available.

bsub -R "swp &gt; 15 && hpux order[ut]" myjob  


You can omit the select keyword (and its square brackets), but if
you include a select section, it must be the first string in the
resource requirement string. If you do not give a section name,
the first resource requirement string is treated as a selection
string (select[_selection\_string_]).

For example, the following resource requirements are equivalent:

bsub -R "type==any order[ut] same[model] rusage[mem=1]" myjob

bsub -R "select[type==any] order[ut] same[model] rusage[mem=1]" myjob

If you need to include a hyphen (-) or other non-alphabetic
characters within the string, enclose the text in single
quotation marks, for example, bsub -R
"select[hname!='host06-x12']".

The selection string must conform to the strict resource
requirement string syntax described in Administering IBM Spectrum
LSF. The strict resource requirement syntax only applies to the
select section. It does not apply to the other resource
requirement sections (order, rusage, same, span, or cu). LSF
rejects resource requirement strings where an rusage section
contains a non-consumable resource.

If **RESRSV\_LIMIT** is set in lsb.queues, the merged
application-level and job-level rusage consumable resource
requirements must satisfy any limits set by **RESRSV\_LIMIT**,
or the job will be rejected.

Any resource for run queue length, such as r15s, r1m or r15m,
specified in the resource requirements refers to the normalized
run queue length.

By default, memory (mem) and swap (swp) limits in
select[] and rusage[] sections are specified in KB. Use the
**LSF\_UNIT\_FOR\_LIMITS** parameter in the lsf.conf file to
specify a larger unit for these limits (MB, GB, TB, PB, EB, or
ZB).

You can use the following units for resource requirements and
limits:

*  KB or K (kilobytes)

*  MB or M (megabytes)

*  GB or G (gigabytes)

*  TB or T (terabytes)

*  PB or P (petabytes)

*  EB or E (exabytes)

*  ZB or Z (zettabytes)

The specified unit is converted to the appropriate value
specified by the **LSF\_UNIT\_FOR\_LIMITS** parameter. The
converted limit values round up to a positive integer. For
resource requirements, you can specify unit for mem,
swp and tmp in select and rusage section.

By default, the tmp resource is not supported by the
**LSF\_UNIT\_FOR\_LIMITS** parameter. Use the parameter
**LSF\_ENABLE\_TMP\_UNIT=Y** to enable the
**LSF\_UNIT\_FOR\_LIMITS** parameter to support limits on the
tmp resource.

If the **LSF\_ENABLE\_TMP\_UNIT=Y** and
**LSF\_UNIT\_FOR\_LIMIT=GB** parameters are set, the following
conversion happens.

bsub -C 500MB -M 1G -S 1TB -F 1M -R "rusage[mem=512MB:swp=1GB:tmp=1TB]" sleep 100 

The units in this job submission are converted to the following
units:

 bsub -C 1 -M 1 -S 1024 -F 1024 -R "rusage[mem=0.5:swp=1:tmp=1024]" sleep 100 


# Multiple Resource Requirement Strings



The bsub command also accepts multiple -R options for the order,
same, rusage (not multi-phase), and select sections. You can
specify multiple strings instead of using the && operator:

bsub -R "select[swp &gt; 15]" -R "select[hpux] order[r15m]"   
-R rusage[mem=100]" -R "order[ut]" -R "same[type]"   
-R "rusage[tmp=50:duration=60]" -R "same[model]" myjob

LSF merges the multiple -R options into one string and selects a
host that meets all of the resource requirements. The number of
-R option sections is unlimited.

**Remember: **Use multiple -R options only with the order,
same, rusage (not multi-phase), and select sections of simple
resource requirement strings and with the bsub and bmod commands.

When application-level and queue-level cu sections are also
defined, the job-level cu section takes precedence and
overwrites the application-level requirement definition, which in
turn takes precedence and overwrites the queue-level requirement
definitions.

For example, when EXCLUSIVE=CU[enclosure] is specified in
the lsb.queues file, with a compute unit type enclosure in
the lsf.params file, and ComputeUnit section in the
lsb.hosts file, the following command submits a job that runs on
64 slots over 4 enclosures or less, and uses the enclosures
exclusively:

bsub -n 64 -R "cu[excl:type=enclosure:maxcus=4]" myjob  


A resource called bigmem is defined in the lsf.shared file
as an exclusive resource for host hostE in the lsf.cluster
file. Use the following command to submit a job that runs on
hostE:

bsub -R "bigmem" myjob  


or

bsub -R "defined(bigmem)" myjob  


A static shared resource is configured for licenses for the
Verilog application as a resource called verilog\_lic. To
submit a job that runs on a host when there is a license
available:

bsub -R "select[defined(verilog_lic)] rusage[verilog_lic=1]" myjob  


The following job requests 20 MB memory for the duration of the
job, and 1 license for 2 minutes:

bsub -R "rusage[mem=20, license=1:duration=2]" myjob  


The following job requests 20 MB of memory, 50 MB of swap space
for 1 hour, and 1 license for 2 minutes:

bsub -R "rusage[mem=20:swp=50:duration=1h, license=1:duration=2]" myjob  


The following job requests 20 MB of memory for the duration of
the job, 50 MB of swap space for 1 hour, and 1 license for 2
minutes.

bsub -R "rusage[mem=20,swp=50:duration=1h, license=1:duration=2]" myjob  


The following job requests 50 MB of swap space, linearly
decreasing the amount reserved over a duration of 2 hours, and
requests 1 license for 2 minutes:

bsub -R "rusage[swp=50:duration=2h:decay=1, license=1:duration=2]" myjob  


The following job requests two resources with same duration but
different decay:

bsub -R "rusage[mem=20:duration=30:decay=1, lic=1:duration=30]" myjob  


The following job uses a multi-phase rusage string to request 50
MB of memory for 10 minutes, followed by 10 MB of memory for the
duration of the job:

bsub -R "rusage[mem=(50 10):duration=(10):decay=(0)]" myjob  


In the following example, you are running an application version
1.5 as a resource called app\_lic\_v15 and the same
application version 2.0.1 as a resource called
app\_lic\_v201. The license key for version 2.0.1 is backward
compatible with version 1.5, but the license key for version 1.5
does not work with 2.0.1.

**Note: **Job-level resource requirement specifications that
use the OR (||) operator take precedence over any queue-level
resource requirement specifications.

*  If you can only run your job using one version of the
   application, submit the job without specifying an alternative
   resource. To submit a job that only uses app\_lic\_v201:

   bsub -R "rusage[app_lic_v201=1]" myjob  


*  If you can run your job using either version of the
   application, try to reserve version 2.0.1 of the application.
   If it is not available, you can use version 1.5. To submit a
   job that tries app\_lic\_v201 before trying
   app\_lic\_v15:

   bsub -R "rusage[app_lic_v201=1||app_lic_v15=1]" myjob  


*  If different versions of an application require different
   system resources, you can specify other resources in your
   rusage strings. To submit a job that uses 20 MB of
   memory for app\_lic\_v201 or 20 MB of memory and 50 MB of
   swap space for app\_lic\_v15:

   bsub -R "rusage[mem=20:app_lic_v15=1||mem=20:swp=50:app_lic_v201=1]" myjob  


You can specify a threshold at which the consumed resource must
be at before an allocation should be made. For example,

bsub -R "rusage[bwidth=1:threshold=5]" myjob  


For example, a job is submitted that consumes 1 unit of bandwidth
(the resource bwidth), but the job should not be scheduled
to run unless the bandwidth on the host is equal to or greater
than 5. In this example, bwidth is a decreasing resource
and the threshold value is interpreted as a floor. If the
resource in question was increasing, then the threshold value
would be interpreted as a ceiling.

An affinity resource requirement string specifies CPU and memory
binding requirements for a resource allocation that is topology
aware. An affinity[] resource requirement section controls the
allocation and distribution of processor units within a host
according to the hardware topology information that LSF collects.


# Resource Reservation Method



Specify the resource reservation method in the resource usage
string by using the **/job**, **/host**, or **/task**
keyword after the numeric value. The resource reservation method
specified in the resource string overrides the global setting
that is specified in the **ReservationUsage** section of the
lsb.resources file. You can only specify resource reservation
methods for consumable resources. Specify the resource
reservation methods as follows:

*  _value_**/job**

   Specifies per-job reservation of the specified resource. This
   is the equivalent of specifying PER\_JOB for the
   **METHOD** parameter in the **ReservationUsage** section
   of the lsb.resources file.

*  _value_**/host**

   Specifies per-host reservation of the specified resource. This
   is the equivalent of specifying PER\_HOST for the
   **METHOD** parameter in the **ReservationUsage** section
   of the lsb.resources file.

*  _value_**/task**

   Specifies per-task reservation of the specified resource. This
   is the equivalent of specifying PER\_TASK for the
   **METHOD** parameter in the **ReservationUsage** section
   of the lsb.resources file.

*  Basic syntax:

   resource_name=value/method:duration=value:decay=value

   For example,

   rusage[mem=10/host:duration=10:decay=0]

*  Multi-phase memory syntax:

   resource_name=(value ...)/method:duration=(value ...):decay=value

   For example,

   rusage[mem=(50 20)/task:duration=(10 5):decay=0]


# Compound Resource Requirements



In some cases different resource requirements may apply to
different parts of a parallel job. The first execution host, for
example, may require more memory or a faster processor for
optimal job scheduling. Compound resource requirements allow you
to specify different requirements for some slots within a job in
the queue-level, application-level, or job-level resource
requirement string.

Compound resource requirement strings can be set by the
application-level or queue-level **RES\_REQ** parameter, or used
with the bsub -R option when a job is submitted. The bmod -R
option accepts compound resource requirement strings for pending
jobs but not running jobs.

Special rules take effect when compound resource requirements are
merged with resource requirements defined at more than one level.
If a compound resource requirement is used at any level (job,
application, or queue) the compound multi-level resource
requirement combinations apply.

Compound resource requirement strings are made up of one or more
simple resource requirement strings as follows:

num1*{simple_string1} + num2*{simple_string2} + ...  


where _numx_ is the number of slots affected and
_simple\_stringx_ is a simple resource requirement string.

The same resource requirement can be used within each component
expression (simple resource requirement). For example, for static
string resource res1 and res2, a resource requirement
such as the following is permitted:

"4*{select[io] same[res1]} + 4*{select[compute] same[res1]}"

With this resource requirement, there are two simple
subexpressions, R1 and R2. For each of these
subexpressions, all slots must come from hosts with equal values
of res1. However, R1 may occupy hosts of a different value
than those occupied by R2.

You can specify a global same requirement that takes effect over
multiple subexpressions of a compound resource requirement
string. For example,

"{4*{select[io]} + 4*{select[compute]}} same[res1]"

This syntax allows users to express that both subexpressions must
reside on hosts that have a common value for res1.

In general, there may be more than two subexpressions in a
compound resource requirement. The global same will apply to all
of them.

Arbitrary nesting of brackets is not permitted. For example, you
cannot have a global same apply to only two of three
subexpressions of a compound resource requirement. However, each
subexpression can have its own local same as well as a global
same for the compound expression as a whole. For example, the
following is permitted:

"{4*{same[res1]} + 4*{same[res1]}} same[res2]"

In addition, a compound resource requirement expression with a
global same may be part of a larger alternative resource
requirement string.

A compound resource requirement expression with a global same can
be used in the following instances:

*  Submitting a job: bsub -R "res_req_string"
   &lt;other_bsub_options&gt; a.out

*  Configuring application profile (lsb.applications file):
   RES_REQ = "res_req_string"

*  Queue configuration (lsb.queues file): RES_REQ =
   "res_req_string"

**Syntax:**

*  A single compound resource requirement:

   "{_compound\_res\_req_} same[_same\_string_]"

*  A compound resource requirement within an alternative resource
   requirement:

   "{{_compound\_res\_req_} same[_same\_string_]} ||
   {_alt\_res\_req_}"

*  A compound resource requirement within an alternative resource
   requirement with delay:

   "{_alt\_res\_req_} || {{_compound\_res\_req_}
   same[_same\_string_]}@_delay_"

   The _delay_ option is a positive integer.

**Restriction: **

*  Compound resource requirements cannot contain the || operator.
   Compound resource requirements cannot be defined (included) in
   any multiple -R options.

*  Compound resource requirements cannot contain the compute unit
   (cu) keywords balance or excl, but works normally with other
   cu keywords (including pref, type, maxcus, and usablecuslots).

*  Resizable jobs can have compound resource requirements, but
   only the portion of the job represented by the last term of
   the compound resource requirement is eligible for automatic
   resizing. When you use the bresize release to release slots,
   you can release only slots represented by the last term of the
   compound resource requirement. To release slots in earlier
   terms, run the bresize release command repeatedly to release
   slots in subsequent last terms.

*  Compound resource requirements cannot be specified in the
   definition of a guaranteed resource pool.

*  Resource allocation for parallel jobs using compound resources
   is done for each compound resource term in the order listed
   instead of considering all possible combinations. A host
   rejected for not satisfying one resource requirement term will
   not be reconsidered for subsequent resource requirement terms.

For jobs without the number of total slots specified with the
bsub -n command, the final _numx_ can be omitted. The final
resource requirement is then applied to the zero or more slots
not yet accounted for:

*  (_final res_req number of slots_) = (total number of
   job slots)-(_num1_+_num2_+ ...)

For jobs with the total number of slots specified with the
bsub -n _num\_slots_ command, the total number of slots
must match the number of slots in the resource requirement:

*  _num\_slots_=(_num1_+_num2_+_num3_+ ...)

For jobs with the minimum and maximum number of slots specified
with the bsub -n _min_, _max_ command, the number
of slots in the compound resource requirement must be compatible
with the minimum and maximum specified.

You can specify the number of slots or processors through the
resource requirement specification. For example, you can specify
a job that requests 10 slots or processors: 1 on a host that has
more than 5000 MB of memory, and an additional 9 on hosts that
have more than 1000 MB of memory:

bsub -R "1*{mem&gt;5000} + 9*{mem&gt;1000}" a.out

To specify compound GPU resource requirements, use the following
keywords and options:

*  In the rusage[] section, use the ngpus_physical resource to
   request the number of physical GPUs, together with the gmodel
   option specify the GPU model, the gmem option to specify the
   amount of reserved GPU memory, the glink option to request
   only GPUs with special connections (xGMI connections for AMD
   GPUs or NVLink connections for Nvidia GPUs), and the mig
   option to specify Nvidia Multi-Instance GPU (MIG) device
   requirements.

*  In the span[] section, use the gtile keyword to specify the
   number of GPUs requested on each socket.

**Note: **If you specify GPU resource requirements with the
bsub -R command option, or the **RES\_REQ** parameter, the LSF
ignores the -gpu option, as well as the **LSB\_GPU\_REQ** and
**GPU\_REQ** parameters.

The following example requests 2 hosts, reserving 2 GPUs, spread
evenly across each socket for the first host, and 4 other hosts
with 1 GPU on each host with 10 GB memory reserved for each GPU:

bsub -R "1*{span[gtile=!] rusage[ngpus_physical=2:gmem=1G]} + 4*{span[ptile=1] rusage[ngpus_physical=1:gmem=10G]}" ./app


# Alternative Resource Requirements



In some circumstances more than one set of resource requirements
may be acceptable for a job to be able to run. LSF provides the
ability to specify alternative resource requirements.

An alternative resource requirement consists of two or more
individual simple or compound resource requirements. Each
separate resource requirement describes an alternative. When a
job is submitted with alternative resource requirements, the
alternative resource picked must satisfy the mandatory first
execution host. If none of the alternatives can satisfy the
mandatory first execution host, the job will PEND.

Alternative resource requirement strings can be specified at the
application-level or queue-level **RES\_REQ** parameter, or used
with bsub -R when a job is submitted. bmod -R also accepts
alternative resource requirement strings for pending jobs.

The rules for merging job, application, and queue alternative
resource requirements are the same as for compound resource
requirements.

Alternative resource requirements cannot be used with the
following features:

*  Multiple bsub -R commands

*  Taskstarter jobs, including those with the tssub command

*  Hosts from HPC integrations that use toplib, including cpuset
   and Blue Gene hosts.

*  Compute unit (cu) sections specified with balance
   or excl keywords.

If a job with alternative resource requirements specified is
re-queued, it will have all alternative resource requirements
considered during scheduling. If a @D delay time is
specified, it is interpreted as waiting, starting from the
original submission time. For a restart job, @D delay time
starts from the restart job submission time.

An alternative resource requirement consists of two or more
individual resource requirements. Each separate resource
requirement describes an alternative. If the resources cannot be
found that satisfy the first resource requirement, then the next
resource requirement is tried, and so on until the requirement is
satisfied.

Alternative resource requirements are defined in terms of a
compound resource requirement, or an atomic resource requirement:

bsub -R "{C1 | R1 } || {C2 | R2 }@D2 || ... || {Cn | Rn }@Dn"   


Where

*  The OR operator (||) separates one alternative resource from
   the next.

*  The _C_ option is a compound resource requirement.

*  The _R_ option is a resource requirement which is the same
   as the current LSF resource requirement, except when there is:

   *  No rusage OR (||).

   *  No compute unit requirement cu[...]

*  The _D_ option is a positive integer:

   *  @_D_ is optional: Do not evaluate the alternative
      resource requirement until _D_ minutes after submission
      time, and requeued jobs still use submission time instead
      of requeue time. There is no D1 because the first
      alternative is always evaluated immediately.

   *  D2 &lt;= D3 &lt;= ... &lt;= D_n_

   *  Not specifying @_D_ means that the alternative
      will be evaluated without delay if the previous alternative
      could not be used to obtain a job's allocation.

For example, you may have a sequential job, but you want
alternative resource requirements (that is, if LSF fails to match
your resource, try another one).

bsub -R "{ select[type==any] order[ut] same[model] rusage[mem=1] } ||    
{ select[type==any] order[ls] same[ostype] rusage[mem=5] }" myjob

You can also add a delay before trying the second alternative:

bsub -R "{ select[type==any] order[ut] same[model] rusage[mem=1] } ||   
{ select[type==any] order[ls] same[ostype] rusage[mem=5] }@4" myjob

You can also have more than 2 alternatives:

bsub -R "{select[type==any] order[ut] same[model] rusage[mem=1] } ||   
{ select[type==any] order[ut] same[model] rusage[mem=1] } ||   
{ select[type==any] order[ut] same[model] rusage[mem=1] }@3 ||   
{ select[type==any] order[ut] same[model] rusage[mem=1] }@6" myjob

Some parallel jobs might need compound resource requirements. You
can specify alternatives for parallel jobs the same way. That is,
you can have several alternative sections each with brace
brackets ({ }) around them separated by ||):

bsub -n 2 -R "{ 1*{ select[type==any] order[ut] same[model] rusage[mem=1]} + 1  
*{ select[type==any] order[ut] same[model] rusage[mem=1] } } ||  
{  1*{ select[type==any] order[ut] same[model] rusage[mem=1]} +   
1*{ select[type==any] order[ut] same[model]  
rusage[mem=1] } }@6" myjob  


Alternatively, the compound resource requirement section can have
both slots requiring the same resource:

bsub -n 2 -R "{ 1*{ select[type==any] order[ut] same[model] rusage[mem=1]}   
+1*{ select[type==any] order[ut] same[model] rusage[mem=1] } } ||   
{  2*{ select[type==any] order[ut] same[model] rusage[mem=1] } }@10" myjob 

An alternative resource requirement can be used to indicate how
many tasks the job requires. For example, a job may request 4
tasks on Solaris host types, or 8 tasks on Linux86 hosts types.
If the -n option is provided at the job level, then the values
specified must be consistent with the values implied by the
resource requirement string:

bsub -R " {8*{type==LINUX86}} || {4*{type==SOLARIS}}" a.out

If they conflict, the job submission is rejected:

bsub -n 3 -R " {8*{type==LINUX86}} || {4*{type==SOLARIS}}" a.out

To specify alternative GPU resource requirements, use the
following keywords and options:

*  In the rusage[] section, use the ngpus_physical resource to
   request the number of physical GPUs, together with the gmodel
   option specify the GPU model, the gmem option to specify the
   amount of reserved GPU memory, the glink option to request
   only GPUs with special connections (xGMI connections for AMD
   GPUs or NVLink connections for Nvidia GPUs), and the mig
   option to specify Nvidia Multi-Instance GPU (MIG) device
   requirements.

*  In the span[] section, use the gtile keyword to specify the
   number of GPUs requested on each socket.

The following example requests 4 hosts with 1 K80 GPU on each
host, or 1 host with 2 P100 GPUs and 1 GPU per socket:

bsub -R "{4*{span[ptile=1] rusage[ngpus_physical=1:gmodel=K80]}} || {1*{span[gtile=1] rusage[ngpus_physical=2:gmodel=P100]}}" ./app

**Related information**  
Assign resource values to be passed into dispatched jobs






**-r**



Reruns a job if the execution host or the system fails; it does
not rerun a job if the job itself fails.




# Categories



properties



# Synopsis



**bsub -r**


# Conflicting Options



Do not use with the -rn option.


# Description



If the execution host becomes unavailable while a job is running,
specifies that the job be rerun on another host. LSF requeues the
job in the same job queue with the same job ID. When an available
execution host is found, reruns the job as if it were submitted
new, even if the job has been checkpointed. You receive a mail
message informing you of the host failure and requeuing of the
job.

If the system goes down while a job is running, specifies that
the job is requeued when the system restarts.

Members of a chunk job can be rerunnable. If the execution host
becomes unavailable, rerunnable chunk job members are removed
from the queue and dispatched to a different execution host.

Interactive jobs (bsub -I) are not rerunnable.

Parent topic: Options






**-rcacct**



Assigns an account name (tag) to hosts that are borrowed through
LSF resource connector, so that the hosts cannot be used by other
user groups, users, or jobs.




# Categories



properties



# Synopsis



**bsub -rcacct "**rc_account_name **"**


# Description



When a job is submitted with a specified account name, hosts that
are borrowed to run this job are tagged with the specified
account name. The borrowed host cannot be used by other jobs that
have a different specified account name (or no specified account
name).

After the borrowed host joins the cluster, use the lshosts -s
command to view the value of the tagged account name.

To use the bsub -rcacct command option,
ENABLE\_RC\_ACCOUNT\_REQUEST\_BY\_USER=Y must be defined in the
lsb.params file.

The account name that is specified here at the job level
overrides the value of the **RC\_ACCOUNT** parameter at the
application and queue levels (lsb.applications and lsb.queues
files), and also overrides the cluster-wide project name that is
set if DEFAULT\_RC\_ACCOUNT\_PER\_PROJECT=Y is defined in the
lsb.params file.

Parent topic: Options






**-rn**



Specifies that the job is never re-runnable.




# Categories



properties



# Synopsis



**bsub -rn**


# Conflicting Options



Do not use with the -r option.


# Description



Disables job rerun if the job was submitted to a re-runnable
queue or application profile with job rerun configured. The
command level job re-runnable setting overrides the application
profile and queue level setting. bsub –rn is different from bmod
-rn, which cannot override the application profile and queue
level re-runnable job setting.

Parent topic: Options






**-rnc**



Specifies the full path of an executable to be invoked on the
first execution host when the job allocation has been modified
(both shrink and grow).




# Categories



properties



# Synopsis



**bsub -rnc** resize_notification_cmd


# Description



The -rnc option overrides the notification command specified in
the application profile (if specified). The maximum length of the
notification command is 4 KB.

Parent topic: Options






**-S**



Sets a per-process (soft) stack segment size limit for each of
the processes that belong to the job.




# Categories



limit



# Synopsis



**-S **stack_limit


# Description



For more information, see getrlimit(2).

By default, the limit is specified in KB. Use
**LSF\_UNIT\_FOR\_LIMITS** in lsf.conf to specify a larger unit
for the limit (MB, GB, TB, PB, or EB).

Parent topic: Options






**-s**



Sends the specified signal when a queue-level run window closes.




# Categories



properties



# Synopsis



**bsub -s **signal


# Description



By default, when the window closes, LSF suspends jobs running in
the queue (job state becomes SSUSP) and stops dispatching jobs
from the queue.

Use -s to specify a signal number; when the run window closes,
the job is signalled by this signal instead of being suspended.

Parent topic: Options






**-sla**



Specifies the service class where the job is to run.




# Categories



properties



# Synopsis



**bsub -sla **service_class_name


# Description



If the SLA does not exist or the user is not a member of the
service class, the job is rejected.

If EGO-enabled SLA scheduling is configured with
**ENABLE\_DEFAULT\_EGO\_SLA** in lsb.params, jobs submitted
without -sla are attached to the configured default SLA.

You can use -g with -sla. All jobs in a job group attached to a
service class are scheduled as SLA jobs. It is not possible to
have some jobs in a job group not part of the service class.
Multiple job groups can be created under the same SLA. You can
submit additional jobs to the job group without specifying the
service class name again. You cannot use job groups with
resource-based SLAs that have guarantee goals.

**Note: **When using the -g with -sla, the job group service
class overrides the service class specified with the -sla option.
For example, if you run bsub -g /g1 -sla sla1 myjob to specify
the /g1 job group with the sla1 service class,

*  If there is no service class attached to the /g1 job
   group, the specified sla1 service class is ignored.

*  If there is a different SLA attached to the /g1 job
   group, the /g1 job group's service class replaces the
   specified sla1 service class.

LSF logs a warning message in the mbatchd log to notify you of
these changes.

**Tip: **Submit your velocity, deadline, and throughput SLA
jobs with a runtime limit (-W option) or specify **RUNLIMIT**
in the queue definition in lsb.queues or **RUNLIMIT** in the
application profile definition in lsb.applications. If you do not
specify a runtime limit for velocity SLAs, LSF automatically
adjusts the optimum number of running jobs according to the
observed run time of finished jobs.

Use bsla to display the properties of service classes configured
in LSB\_CONFDIR/_cluster\_name_/configdir/lsb.serviceclasses
(see lsb.serviceclasses) and dynamic information about the state
of each service class.


# Examples



bsub -W 15 -sla Duncan sleep 100

Submit the UNIX command sleep together with its argument
100 as a job to the service class named Duncan.

The example submits and IBM PE job and assumes two hosts in
cluster, hostA and hostB, each with 4 cores and 2
networks. Each network has one IB adapter with 64 windows.

Parent topic: Options






**-sp**



Specifies user-assigned job priority that orders jobs in a queue.




# Categories



properties



# Synopsis



**bsub -sp **priority


# Description



Valid values for priority are any integers between 1 and the
value of the **MAX\_USER\_PRIORITY** parameter that is configured
in the lsb.params file. Job priorities that are not valid are
rejected. LSF administrators and queue administrators can specify
priorities beyond **MAX\_USER\_PRIORITY** for any jobs in the
queue.

Job owners can change the priority of their own jobs relative to
all other jobs in the queue. LSF administrators and queue
administrators can change the priority of all jobs in a queue.

Job order is the first consideration to determine job eligibility
for dispatch. Jobs are still subject to all scheduling policies
regardless of job priority. Jobs are scheduled based first on
their queue priority first, then job priority, and lastly in
first-come first-served order.

User-assigned job priority can be configured with automatic job
priority escalation to automatically increase the priority of
jobs that are pending for a specified period
(**JOB\_PRIORITY\_OVER\_TIME** in lsb.params).

When absolute priority scheduling is configured in the submission
queue (the **APS\_PRIORITY** parameter in the lsb.queues file),
the user-assigned job priority is used for the JPRIORITY
factor in the APS calculation.

**Note: **If you enable the **RELAX\_JOB\_DISPATCH\_ORDER**
parameter in the lsb.params file, which allows LSF to deviate
from standard job prioritization policies, LSF might break the
job dispatch order as specified by the user priority.

Parent topic: Options






**-stage**



Specifies the options for direct data staging (for example, IBM
CAST burst buffer).




# Categories



properties



# Synopsis



**bsub -stage "** [**storage=** min_size [ **,** max_size ]
] [**:in=**path_to_stage_in_script ]
[**:out=**path_to_stage_out_script ]"


# Description



The -stage option uses the following keywords:

**storage=min\_size[,max\_size]**  
         Specifies the minimum required and (optionally) maximum
         available storage space on the allocation.

**in=file\_path**  
         Specifies the file path to the user stage in script.
         This is launched by the stage in script as specified by
         the **LSF\_STAGE\_IN\_EXEC** parameter in the lsf.conf
         file.

**out=file\_path**  
         Specifies the file path to the user stage out script.
         This is launched by the stage out script as specified by
         the **LSF\_STAGE\_OUT\_EXEC** parameter in the lsf.conf
         file.

You can specify one or more keywords. Use a colon (:) to
separate multiple keywords.


# Example



bsub -stage
"storage=5:in=/u/usr1/mystagein.pl:out=/home/mystagein.pl" -q bbq
myjob

Parent topic: Options






**-step\_cgroup**



Enables the job to create a cgroup for each job step.




# Categories



properties



# Synopsis



**bsub -step_cgroup y** | **n**


# Description



This option is for LSF jobs that are submitted with the IBM
Cluster Systems Manager (CSM) integration package.


# Default



n. The job does not create a cgroup for each job step.

Parent topic: Options






**-T**



Sets the limit of the number of concurrent threads to the
specified value for the whole job.




# Categories



limit



# Synopsis



**bsub -T **thread_limit


# Description



The default is no limit.

Exceeding the limit causes the job to terminate. The system sends
the following signals in sequence to all processes belongs to the
job: SIGINT, SIGTERM, and SIGKILL.


# Examples



bsub -T 4 myjob

Submits myjob with a maximum number of concurrent threads
of 4.

Parent topic: Options






**-t**



Specifies the job termination deadline.




# Categories



schedule



# Synopsis



**bsub -t** [[[year:]month**:**]day**:**]hour**:**minute


# Description



If a UNIX job is still running at the termination time, the job
is sent a SIGUSR2 signal, and is killed if it does not terminate
within ten minutes.

If a Windows job is still running at the termination time, it is
killed immediately. (For a detailed description of how these jobs
are killed, see bkill.)

In the queue definition, a TERMINATE action can be configured to
override the bkill default action (see the **JOB\_CONTROLS**
parameter in lsb.queues(5)).

In an application profile definition, a **TERMINATE\_CONTROL**
action can be configured to override the bkill default action
(see the **TERMINATE\_CONTROL** parameter in
lsb.applications(5)).

The format for the termination time is
[[_year_:][_month_:]_day_:]_hour_:_minute_
where the number ranges are as follows: year after 1970, month
1-12, day 1-31, hour 0-23, minute 0-59.

At least two fields must be specified. These fields are assumed
to be _hour_:_minute_. If three fields are given,
they are assumed to be
_day_:_hour_:_minute_, four fields are
assumed to be
_month_:_day_:_hour_:_minute_
and five fields are assumed to be _year_:
_month_:_day_:_hour_:_minute_.

If the year field is specified and the specified time is in the
past, the job submission request is rejected.

Parent topic: Options






**-ti**



Enables automatic orphan job termination at the job level for a
job with a dependency expression (set using -w).




# Categories



schedule



# Synopsis



**bsub -w '**dependency\_expression**'**[**-ti**]


# Conflicting Options



Use only with the -w option.


# Description



-ti is a sub-option of -w. With this sub-option, the
cluster-level orphan job termination grace period is ignored (if
configured) and the job can be terminated as soon as it is found
to be an orphan. This option is independent of the cluster-level
configuration. Therefore, if the LSF administrator did not enable
**ORPHAN\_JOB\_TERM\_GRACE\_PERIOD** at the cluster level, you can
still use automatic orphan job termination on a per-job basis.

Parent topic: Options






**-tty**



When submitting an interactive job, displays output/error
messages on the screen (except pre-execution output/error
messages).




# Categories



io



# Synopsis



**bsub -I** | **-Ip** | **-IS** | **-ISp** | **-ISs** |
**-Is** | **-IX** [**-tty**]


# Conflicting Options



Use only with the following options: -I, -Ip, -IS, -ISp, -ISs,
-Is, -IX.


# Description



-tty is a sub-option of the interactive job submission options
(including -I, -Ip, -IS, -ISp, -ISs, -Is, and -IX).

Parent topic: Options






**-U**



If an advance reservation has been created with the brsvadd
command, the job makes use of the reservation.




# Categories



resource, schedule



# Synopsis



**bsub -U **reservation_ID


# Description



For example, if the following command was used to create the
reservation user1#0:

brsvadd -n 1024 -m hostA -u user1 -b 13:0 -e 18:0  
Reservation "user1#0" is created  


The following command uses the reservation:

bsub -U user1#0 myjob  


The job can only use hosts reserved by the reservation
user1#0. LSF only selects hosts in the reservation. You can
use the -m option to specify particular hosts within the list of
hosts reserved by the reservation, but you cannot specify other
hosts not included in the original reservation.

If you do not specify hosts (bsub -m) or resource requirements
(bsub -R), the default resource requirement is to select hosts
that are of any host type (LSF assumes "type==any" instead of
"type==local" as the default select string).

If you later delete the advance reservation while it is still
active, any pending jobs still keep the "type==any" attribute.

A job can only use one reservation. There is no restriction on
the number of jobs that can be submitted to a reservation;
however, the number of slots available on the hosts in the
reservation may run out. For example, reservation user2#0
reserves 128 slots on hostA. When all 128 slots on
hostA are used by jobs referencing user2#0,
hostA is no longer available to other jobs using
reservation user2#0. Any single user or user group can have
a maximum of 100 reservation IDs.

Jobs referencing the reservation are killed when the reservation
expires. LSF administrators can prevent running jobs from being
killed when the reservation expires by changing the termination
time of the job using the reservation (bmod -t) before the
reservation window closes.

To use an advance reservation on a remote host, submit the job
and specify the remote advance reservation ID. For example:

bsub -U user1#01@cluster1  


In this example, it is assumed that the default queue is
configured to forward jobs to the remote cluster.

Parent topic: Options






**-u**



Sends mail to the specified email destination.




# Categories



notify



# Synopsis



**bsub -u **mail_user

**bsub -u "**user@example.com ... **"**


# Description



The email destination can be an email address or an OS user
account. To specify a Windows user account, include the domain
name in uppercase letters and use a single backslash
(_DOMAIN\_NAME_\_user\_name_) in a Windows command line or
a double backslash (_DOMAIN\_NAME_\\_user\_name_) in a UNIX
command line.

To specify multiple email addresses, use quotation marks and
separate each address with a space. The total length of the
address string cannot be longer than 511 characters.

Parent topic: Options






**-ul**



Passes the current operating system user shell limits for the job
submission user to the execution host.




# Categories



limit



# Synopsis



**bsub -ul**


# Description



User limits cannot override queue hard limits. If user limits
exceed queue hard limits, the job is rejected.

**Restriction: **UNIX and Linux only. -ul is not supported on
Windows.

The following bsub options for job-level runtime limits override
the value of the user shell limits:

*  Per-process (soft) core file size limit (-C)

*  CPU limit (-c)

*  Per-process (soft) data segment size limit (-D)

*  File limit (-F)

*  Per-process (soft) memory limit (-M)

*  Process limit (-p)

*  Per-process (soft) stack segment size limit (-S)

*  Limit of the number of concurrent threads (-T)

*  Total process virtual memory (swap space) limit (-v)

*  Runtime limit (-W)

LSF collects the user limit settings from the user's running
environment that are supported by the operating system, and sets
the value to submission options if the value is not unlimited. If
the operating system has other kinds of shell limits, LSF does
not collect them. LSF collects the following operating system
user limits:

*  CPU time in milliseconds

*  Maximum file size

*  Data size

*  Stack size

*  Core file size

*  Resident set size

*  Open files

*  Virtual (swap) memory

*  Process limit

*  Thread limit

Parent topic: Options






**-v**



Sets the total process virtual memory limit to the specified
value for the whole job.




# Categories



limit



# Synopsis



**bsub -v **swap_limit


# Description



The default is no limit. Exceeding the limit causes the job to
terminate.

By default, the limit is specified in KB. Use
**LSF\_UNIT\_FOR\_LIMITS** in lsf.conf to specify a larger unit
for the limit (MB, GB, TB, PB, or EB).

Parent topic: Options






**-W**



Sets the runtime limit of the job.




# Categories



limit



# Synopsis



**bsub -W** [hour**:**]minute[**/**host_name |
**/**host_model]


# Description



If a UNIX job runs longer than the specified run limit, the job
is sent a SIGUSR2 signal, and is killed if it does not terminate
within ten minutes. If a Windows job runs longer than the
specified run limit, it is killed immediately. (For a detailed
description of how these jobs are killed, see bkill.)

In the queue definition, a **TERMINATE** action can be
configured to override the bkill default action (see the
**JOB\_CONTROLS** parameter in lsb.queues).

In an application profile definition, a **TERMINATE\_CONTROL**
action can be configured to override the bkill default action
(see the **TERMINATE\_CONTROL** parameter in lsb.applications).

If you want to provide LSF with an estimated run time without
killing jobs that exceed this value, submit the job with -We, or
define the **RUNTIME** parameter in lsb.applications and submit
the job to that application profile. LSF uses the estimated
runtime value for scheduling purposes only.

The run limit is in the form of [_hour_:]_minute_.
The minutes can be specified as a number greater than 59. For
example, three and a half hours can either be specified as 3:30,
or 210.

The run limit you specify is the normalized run time. This is
done so that the job does approximately the same amount of
processing, even if it is sent to host with a faster or slower
CPU. Whenever a normalized run time is given, the actual time on
the execution host is the specified time multiplied by the CPU
factor of the normalization host then divided by the CPU factor
of the execution host.

If **ABS\_RUNLIMIT=Y** is defined in lsb.params, the runtime
limit and the runtime estimate are not normalized by the host CPU
factor. Absolute wall-clock run time is used for all jobs
submitted with a runtime limit or runtime estimate.

Optionally, you can supply a host name or a host model name
defined in LSF. You must insert ‘/' between the run limit
and the host name or model name.

If no host or host model is given, LSF uses the default runtime
normalization host defined at the queue level
(**DEFAULT\_HOST\_SPEC** in lsb.queues) if it has been
configured; otherwise, LSF uses the default CPU time
normalization host defined at the cluster level
(**DEFAULT\_HOST\_SPEC** in lsb.params) if it has been
configured; otherwise, LSF uses the submission host.

For LSF multicluster capability jobs, if no other CPU time
normalization host is defined and information about the
submission host is not available, LSF uses the host with the
largest CPU factor (the fastest host in the cluster).

If the job also has termination time specified through the bsub
-t option, LSF determines whether the job can actually run for
the specified length of time allowed by the run limit before the
termination time. If not, then the job is aborted.

If the **IGNORE\_DEADLINE** parameter is set in lsb.queues, this
behavior is overridden and the run limit is ignored.

Jobs submitted to a chunk job queue are not chunked if the run
limit is greater than 30 minutes.


# Examples



bsub -W 15 -sla Duncan sleep 100

Submit the UNIX command sleep together with its argument
100 as a job to the service class named Duncan.

Parent topic: Options






**-w**



LSF does not place your job unless the dependency expression
evaluates to TRUE.




# Categories



schedule



# Synopsis



**bsub -w '**dependency\_expression**'**[**-ti**]


# Description



**-w 'dependency_expression' [-ti]**  
         If you specify a dependency on a job that LSF cannot
         find (such as a job that has not yet been submitted),
         your job submission fails.

         The dependency expression is a logical expression
         composed of one or more dependency conditions. To make
         dependency expression of multiple conditions, use the
         following logical operators:

         && (AND)

         || (OR)

         ! (NOT)

         Use parentheses to indicate the order of operations, if
         necessary.

         Enclose the dependency expression in single quotes (')
         to prevent the shell from interpreting special
         characters (space, any logic operator, or parentheses).
         If you use single quotes for the dependency expression,
         use double quotes (") for quoted items within it, such
         as job names.

         In a Windows environment with multiple job dependencies,
         use only double quotes.

         In dependency conditions, job names specify only your
         own jobs. By default, if you use the job name to specify
         a dependency condition, and more than one of your jobs
         has the same name, all of your jobs that have that name
         must satisfy the test. If **JOB\_DEP\_LAST\_SUB** in
         lsb.params is set to 1, the test is done on the job
         submitted most recently.

         Use double quotes (") around job names that begin with a
         number. In the job name, specify the wildcard character
         asterisk (*) at the end of a string, to indicate all
         jobs whose name begins with the string. For example, if
         you use jobA* as the job name, it specifies jobs
         named jobA, jobA1, jobA\_test,
         jobA.log, etc.

         Use the * with dependency conditions to define
         one-to-one dependency among job array elements such that
         each element of one array depends on the corresponding
         element of another array. The job array size must be
         identical.

         For example:

         bsub -w "done(myarrayA[*])" -J "myArrayB[1-10]" myJob2   


         indicates that before element 1 of myArrayB can
         start, element 1 of myArrayA must be completed,
         and so on.

         You can also use the * to establish one-to-one array
         element dependencies with bmod after an array has been
         submitted.

         If you want to specify array dependency by array name,
         set **JOB\_DEP\_LAST\_SUB** in lsb.params. If you do not
         have this parameter set, the job is rejected if one of
         your previous arrays has the same name but a different
         index.

         In dependency conditions, the variable _op_
         represents one of the following relational operators:

         &gt;

         &gt;=

         &lt;

         &lt;=

         ==

         !=

         Use the following conditions to form the dependency
         expression. Where _job\_name_ is mentioned, LSF
         refers to the oldest job of _job\_name_ in memory.

         **done(job_ID |"job_name" ...) **  
                  The job state is DONE.

         **ended(job_ID | "job_name") **  
                  The job state is EXIT or DONE.

         exit(job_ID | "job_name"
         [,[operator**] exit\_code])**  
                  The job state is EXIT, and the job's exit code
                  satisfies the comparison test.

                  If you specify an exit code with no operator,
                  the test is for equality (== is assumed).

                  If you specify only the job, any exit code
                  satisfies the test.

         external(job_ID | "job_name",
         "status\_text**")**  
                  The job has the specified job status. (Commands
                  bstatus and bpost set, change, and retrieve
                  external job status messages.)

                  If you specify the first word of the job status
                  description (no spaces), the text of the job's
                  status begins with the specified word. Only the
                  first word is evaluated.

         **job_ID | "job\_name"**  
                  If you specify a job without a dependency
                  condition, the test is for the DONE state (LSF
                  assumes the “done” dependency condition by
                  default).

         numdone(job_ID, operator number |
         *)  
                  For a job array, the number of jobs in the DONE
                  state satisfies the test. Use * (with no
                  operator) to specify all the jobs in the array.

         numended(job_ID, operator number |
         *)  
                  For a job array, the number of jobs in the DONE
                  or EXIT states satisfies the test. Use * (with
                  no operator) to specify all the jobs in the
                  array.

         numexit(job_ID, operator number |
         *)  
                  For a job array, the number of jobs in the EXIT
                  state satisfies the test. Use * (with no
                  operator) to specify all the jobs in the array.

         numhold(job_ID, operator number |
         *)  
                  For a job array, the number of jobs in the
                  PSUSP state satisfies the test. Use * (with no
                  operator) to specify all the jobs in the array.

         numpend(job_ID, operator number |
         *)  
                  For a job array, the number of jobs in the PEND
                  state satisfies the test. Use * (with no
                  operator) to specify all the jobs in the array.

         numrun(job_ID, operator number |
         *)  
                  For a job array, the number of jobs in the RUN
                  state satisfies the test. Use * (with no
                  operator) to specify all the jobs in the array.

         numstart(job_ID, operator number |
         *)  
                  For a job array, the number of jobs in the RUN,
                  USUSP, or SSUSP states satisfies the test. Use
                  * (with no operator) to specify all the jobs in
                  the array.

         **post\_done(job_ID | "job\_name")**  
                  The job state is POST_DONE (post-execution
                  processing of the specified job has completed
                  without errors).

         **post\_err(job_ID | "job\_name")**  
                  The job state is POST_ERR (post-execution
                  processing of the specified job has completed
                  with errors).

         **started(job_ID | "job\_name")**  
                  The job state is:

                  *  USUSP, SSUSP, DONE, or EXIT

                  *  RUN and the job has a pre-execution command
                     (bsub -E) that is done.

Parent topic: Options






**-wa**



Specifies the job action to be taken before a job control action
occurs.




# Categories



properties



# Synopsis



**bsub -wa '**signal**'**


# Description



A job warning action must be specified with a job action warning
time in order for job warning to take effect.

If -wa is specified, LSF sends the warning action to the job
before the actual control action is taken. This allows the job
time to save its result before being terminated by the job
control action.

The warning action specified by -wa option overrides
**JOB\_WARNING\_ACTION** in the queue. **JOB\_WARNING\_ACTION**
is used as the default when no command line option is specified.


# Examples



The following specifies that 2 minutes before the job reaches its
runtime limit, a URG signal is sent to the job:

bsub -W 60 -wt '2' -wa 'URG' myjob

Parent topic: Options






**-We**



Specifies an estimated run time for the job.



# Categories



limit



# Synopsis



**bsub -We** [hour**:**]minute[**/**host_name |
**/**host_model]


# Description



LSF uses the estimated value for job scheduling purposes only,
and does not kill jobs that exceed this value unless the jobs
also exceed a defined runtime limit. The format of runtime
estimate is same as run limit set by the -W option.

Use **JOB\_RUNLIMIT\_RATIO** in lsb.params to limit the runtime
estimate users can set. If **JOB\_RUNLIMIT\_RATIO** is set to 0
no restriction is applied to the runtime estimate.

The job-level runtime estimate setting overrides the
**RUNTIME** setting in an application profile in
lsb.applications.

Parent topic: Options






**-wt**



Specifies the amount of time before a job control action occurs
that a job warning action is to be taken.




# Categories



properties



# Synopsis



**bsub -wt '**[hour**:**]minute**'**


# Description



Job action warning time is not normalized.

A job action warning time must be specified with a job warning
action in order for job warning to take effect.

The warning time specified by the bsub -wt option overrides
**JOB\_ACTION\_WARNING\_TIME** in the queue.
**JOB\_ACTION\_WARNING\_TIME** is used as the default when no
command line option is specified.


# Examples



The following specifies that 2 minutes before the job reaches its
runtime limit, an URG signal is sent to the job:

bsub -W 60 -wt '2' -wa 'URG' myjob

Parent topic: Options






**-x**



Puts the host running your job into exclusive execution mode.




# Categories



schedule



# Synopsis



**bsub -x**


# Description



In exclusive execution mode, your job runs by itself on a host.
It is dispatched only to a host with no other jobs running, and
LSF does not send any other jobs to the host until the job
completes.

To submit a job in exclusive execution mode, the queue must be
configured to allow exclusive jobs.

When the job is dispatched, bhosts reports the host status as
closed_Excl, and lsload reports the host status as lockU.

Until your job is complete, the host is not selected by LIM in
response to placement requests made by lsplace, lsrun or lsgrun
or any other load sharing applications.

You can force other jobs to run on the host by using the -m
_host\_name_ option of brun(1) to explicitly specify the
locked host.

You can force LIM to run other interactive jobs on the host by
using the -m _host\_name_ option of lsrun or lsgrun to
explicitly specify the locked host.

Parent topic: Options






**-XF**



Submits a job using SSH X11 forwarding.




# Categories



properties



# Synopsis



**bsub -XF**


# Conflicting Options



Do not use with the following options: -IX, -K, -r.


# Description



A job submitted with SSH X11 forwarding cannot be used with job
arrays, job chunks, or user account mapping.

Jobs with SSH X11 forwarding cannot be checked or modified by an
esub.

Use -XF with -I to submit an interactive job using SSH X11
forwarding. The session is displayed throughout the job
lifecycle.

**Note: **LSF does not support SSH X11 forwarding with the LSF
multicluster capability.

bjobs -l displays job information, including any jobs submitted
with SSH X11 forwarding.

bhist -l displays historical job information, including any jobs
submitted with SSH X11 forwarding.

Optionally, specify **LSB\_SSH\_XFORWARD\_CMD** in lsf.conf. You
can replace the default value with an SSH command (full PATH and
options allowed).

For more information about **LSB\_SSH\_XFORWARD\_CMD**, see the
LSF Configuration Reference.


# Troubleshooting



Enable the following parameters in lsf.conf::

*  **LSF\_NIOS\_DEBUG=1**

*  **LSF\_LOG\_MASK="LOG\_DEBUG"**

SSH X11 forwarding must be already working outside LSF.

Parent topic: Options






**-yaml**



Submits a job using a YAML file to specify job submission
options.




# Categories



properties



# Synopsis



**bsub -yaml** file_name


# Conflicting Options



Do not use with the following options: -json, -jsdl,
-jsdl_strict.


# Description



In the YAML file, specify the bsub option name or alias and the
value as the key-value pair. To specify job command or job
script, use the command option name with the name of the
command or job script as the value. For options that have no
values (flags), use null or (for string type options) an
empty value. Specify the key-value pairs under the category name
of the option.

If you specify duplicate or conflicting job submission
parameters, LSF resolves the conflict by applying the following
rules:

1. The parameters that are specified in the command line override
   all other parameters.

2. The parameters that are specified in the YAML file override
   the job script.


# Job Submission Options and Aliases



The following is a list of the bsub options to use in the file.
You can use either the option name without a hyphen, or the
alias. For example, to use the bsub -app option, specify either
**appName** or **app** as the key name, and the application
profile name as the key value.

**Table 1. List of supported bsub options and aliases**

+---------------+---------------+---------------+---------------+
| Option        | Alias         | Category      | Type          |
+---------------+---------------+---------------+---------------+
| a             | appSpecific   | schedule      | String        |
+---------------+---------------+---------------+---------------+
| alloc_flags   | allocFlags    | resource      | String        |
+---------------+---------------+---------------+---------------+
| app           | appName       | properties    | String        |
+---------------+---------------+---------------+---------------+
| ar            | autoResize    | properties    | String        |
+---------------+---------------+---------------+---------------+
| B             | notifyJobDisp | notify        | String        |
|               | atch          |               |               |
+---------------+---------------+---------------+---------------+
| b             | specifiedStar | schedule      | String        |
|               | tTime         |               |               |
+---------------+---------------+---------------+---------------+
| C             | coreLimit     | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| c             | cpuTimeLimit  | limit         | String        |
+---------------+---------------+---------------+---------------+
| clusters      | clusters      | resource      | String        |
|               |               | schedule      |               |
+---------------+---------------+---------------+---------------+
| cn_cu         | computeNodeCo | resource      | String        |
|               | mputeUnit     |               |               |
+---------------+---------------+---------------+---------------+
| cn_mem        | computeNodeMe | resource      | Numeric       |
|               | m             |               |               |
+---------------+---------------+---------------+---------------+
| core_isolatio | coreIsolation | resource      | Numeric       |
| n             |               |               |               |
+---------------+---------------+---------------+---------------+
| csm           | csm           | properties    | String        |
+---------------+---------------+---------------+---------------+
| cwd           | cwd           | io            | String        |
+---------------+---------------+---------------+---------------+
| D             | dataLimit     | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| data          | data          | resource      | String array  |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| datachk       | dataCheck     | resource      | String        |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| datagrp       | dataGroup     | resource      | String        |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| E             | preExecCmd    | properties    | String        |
+---------------+---------------+---------------+---------------+
| Ep            | psotExecCmd   | properties    | String        |
+---------------+---------------+---------------+---------------+
| e             | errorAppendFi | io            | String        |
|               | le            |               |               |
+---------------+---------------+---------------+---------------+
| env           | envVariable   | properties    | String        |
+---------------+---------------+---------------+---------------+
| eo            | errorOverwrit | io            | String        |
|               | eFile         |               |               |
+---------------+---------------+---------------+---------------+
| eptl          | epLimitRemain | limit         | String        |
+---------------+---------------+---------------+---------------+
| ext           | extSched      | schedule      | String        |
+---------------+---------------+---------------+---------------+
| F             | fileLimit     | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| f             | file          | io            | String        |
+---------------+---------------+---------------+---------------+
| freq          | frequency     | resource      | Numeric       |
+---------------+---------------+---------------+---------------+
| G             | userGroup     | schedule      | String        |
+---------------+---------------+---------------+---------------+
| g             | jobGroupName  | properties    | String        |
+---------------+---------------+---------------+---------------+
| gpu           | gpu           | resource      | String        |
+---------------+---------------+---------------+---------------+
| H             | hold          | schedule      | String        |
+---------------+---------------+---------------+---------------+
| hl            | hostLimit     | limit         | String        |
+---------------+---------------+---------------+---------------+
| hostfile      | hostFile      | resource      | String        |
+---------------+---------------+---------------+---------------+
| I             | interactive   | properties    | String        |
+---------------+---------------+---------------+---------------+
| Ip            | interactivePt | properties    | String        |
|               | y             |               |               |
+---------------+---------------+---------------+---------------+
| IS            | interactiveSs | properties    | String        |
|               | h             |               |               |
+---------------+---------------+---------------+---------------+
| ISp           | interactiveSs | properties    | String        |
|               | hPty          |               |               |
+---------------+---------------+---------------+---------------+
| ISs           | interactiveSs | properties    | String        |
|               | hPtyShell     |               |               |
+---------------+---------------+---------------+---------------+
| Is            | interactivePt | properties    | String        |
|               | yShell        |               |               |
+---------------+---------------+---------------+---------------+
| IX            | interactiveXW | properties    | String        |
|               | in            |               |               |
+---------------+---------------+---------------+---------------+
| i             | inputFile     | io            | String        |
+---------------+---------------+---------------+---------------+
| is            | inputHandleFi | io            | String        |
|               | le            |               |               |
+---------------+---------------+---------------+---------------+
| J             | jobName       | properties    | String        |
+---------------+---------------+---------------+---------------+
| Jd            | jobDescriptio | properties    | String        |
|               | n             |               |               |
+---------------+---------------+---------------+---------------+
| jsm           | jobStepManage | properties    | String        |
|               | r             |               |               |
+---------------+---------------+---------------+---------------+
| K             | jobWaitDone   | notify        | String        |
|               |               | properties    |               |
+---------------+---------------+---------------+---------------+
| k             | checkPoint    | properties    | String        |
+---------------+---------------+---------------+---------------+
| L             | loginShell    | properties    | String        |
+---------------+---------------+---------------+---------------+
| Lp            | licenseProjec | schedule      | String        |
|               | tName         |               |               |
+---------------+---------------+---------------+---------------+
| ln_mem        | launchNodeMem | resource      | Numeric       |
+---------------+---------------+---------------+---------------+
| ln_slots      | launchNodeSlo | resource      | Numeric       |
|               | ts            |               |               |
+---------------+---------------+---------------+---------------+
| M             | memLimit      | limit         | String        |
+---------------+---------------+---------------+---------------+
| m             | machines      | resource      | String        |
+---------------+---------------+---------------+---------------+
| mig           | migThreshold  | schedule      | Numeric       |
+---------------+---------------+---------------+---------------+
| N             | notifyJobDone | notify        | String        |
+---------------+---------------+---------------+---------------+
| Ne            | notifyJobExit | notify        | String        |
+---------------+---------------+---------------+---------------+
| n             | numTasks      | resource      | String        |
+---------------+---------------+---------------+---------------+
| notify        | notifyJobAny  | notify        | String        |
+---------------+---------------+---------------+---------------+
| network       | networkReq    | resource      | String        |
+---------------+---------------+---------------+---------------+
| nnodes        | numNodes      | resource      | Numeric       |
+---------------+---------------+---------------+---------------+
| o             | outputAppendF | io            | String        |
|               | ile           |               |               |
+---------------+---------------+---------------+---------------+
| oo            | outputOverwri | io            | String        |
|               | teFile        |               |               |
+---------------+---------------+---------------+---------------+
| outdir        | outputDir     | io            | String        |
+---------------+---------------+---------------+---------------+
| P             | projectName   | properties    | String        |
+---------------+---------------+---------------+---------------+
| p             | processLimit  | limit         | String        |
+---------------+---------------+---------------+---------------+
| pack          | pack          | pack          | String        |
+---------------+---------------+---------------+---------------+
| ptl           | pendTimeLimit | limit         | String        |
+---------------+---------------+---------------+---------------+
| Q             | exitCode      | properties    | String        |
+---------------+---------------+---------------+---------------+
| q             | queueName     | properties    | String        |
+---------------+---------------+---------------+---------------+
| R             | resReq        | resource      | String array  |
+---------------+---------------+---------------+---------------+
| r             | rerun         | properties    | String        |
+---------------+---------------+---------------+---------------+
| rn            | rerunNever    | properties    | String        |
+---------------+---------------+---------------+---------------+
| rnc           | resizeNotifCm | properties    | String        |
|               | d             |               |               |
+---------------+---------------+---------------+---------------+
| S             | stackLimit    | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| s             | signal        | properties    | Numeric       |
+---------------+---------------+---------------+---------------+
| sla           | serviceClassN | properties    | String        |
|               | ame           |               |               |
+---------------+---------------+---------------+---------------+
| smt           | smt           | properties    | Numeric       |
+---------------+---------------+---------------+---------------+
| sp            | jobPriority   | properties    | Numeric       |
+---------------+---------------+---------------+---------------+
| stage         | stage         | properties    | String        |
+---------------+---------------+---------------+---------------+
| step_cgroup   | stepCgroup    | properties    | String        |
+---------------+---------------+---------------+---------------+
| T             | threadLimit   | limit         | Numeric       |
+---------------+---------------+---------------+---------------+
| t             | specifiedTerm | schedule      | String        |
|               | inateTime     |               |               |
+---------------+---------------+---------------+---------------+
| ti            | terminateInde | schedule      | String        |
|               | pend          |               |               |
+---------------+---------------+---------------+---------------+
| tty           | tty           | io            | String        |
+---------------+---------------+---------------+---------------+
| U             | rsvId         | schedule      | String        |
|               |               | resource      |               |
+---------------+---------------+---------------+---------------+
| u             | mailUser      | notify        | String        |
+---------------+---------------+---------------+---------------+
| ul            | userLimit     | limit         | String        |
+---------------+---------------+---------------+---------------+
| v             | swapLimit     | limit         | String        |
+---------------+---------------+---------------+---------------+
| W             | runtimeLimit  | limit         | String        |
+---------------+---------------+---------------+---------------+
| We            | estimatedRunT | limit         | String        |
|               | ime           |               |               |
+---------------+---------------+---------------+---------------+
| w             | dependency    | schedule      | String        |
+---------------+---------------+---------------+---------------+
| wa            | warningAction | properties    | String        |
+---------------+---------------+---------------+---------------+
| wt            | warningTime   | properties    | String        |
+---------------+---------------+---------------+---------------+
| XF            | x11Forward    | properties    | String        |
+---------------+---------------+---------------+---------------+
| x             | exclusive     | schedule      | String        |
+---------------+---------------+---------------+---------------+
| Zs            | jobSpool      | properties    | String        |
|               |               | script        |               |
+---------------+---------------+---------------+---------------+
| h             | help          |               | String        |
+---------------+---------------+---------------+---------------+
| V             | version       |               | String        |
+---------------+---------------+---------------+---------------+
| command       | cmd           |               | String        |
| The job       |               |               |               |
| command with  |               |               |               |
| arguments or  |               |               |               |
| job script.   |               |               |               |
+---------------+---------------+---------------+---------------+

For more information on the syntax of the key values to specify
for each option, refer to the description of each bsub option in
the IBM Spectrum LSF Command Reference.


# Example



For the following job submission command:

bsub -r -H -N -Ne -i /tmp/input/jobfile.sh -outdir /tmp/output -C 5 -c 2022:12:12 -cn_mem 256 -hostfile /tmp/myHostFile.txt -q normal -G myUserGroup -u "user@example.com" myjob

The following YAML file specifies the equivalent job submission
command:

io:   
    inputFile: /tmp/input/jobfile.sh  
    outputDir: /tmp/output  
limit:   
    coreLimit: 5  
    cpuTimeLimit: 2022:12:12  
resource:   
    computeNodeMem: 256  
    hostFile: /tmp/myHostFile.txt  
properties:  
    queueName: normal  
    rerun: null  
schedule:   
    hold: ""  
    userGroup: myUserGroup  
notify:   
    notifyJobDone: ""  
    notifyJobExit:   
    mailUser: user@example.com  
command: myjob

Parent topic: Options






**-Zs**



Spools a job command file to the directory specified by the
**JOB\_SPOOL\_DIR** parameter in lsb.params, and uses the spooled
file as the command file for the job.




# Categories



properties, script



# Synopsis



**bsub -Zs**


# Description



By default, the command file is spooled to
LSB\_SHAREDIR/_cluster\_name_/lsf_cmddir. If the lsf_cmddir
directory does not exist, LSF creates it before spooling the
file. LSF removes the spooled file when the job completes.

If **JOB\_SPOOL\_DIR** is specified, the -Zs option spools the
command file to the specified directory and uses the spooled file
as the input file for the job.

**JOB\_SPOOL\_DIR** can be any valid path up to a maximum length
up to 4094 characters on UNIX and Linux or up to 255 characters
for Windows.

**JOB\_SPOOL\_DIR** must be readable and writable by the job
submission user, and it must be shared by the management host and
the submission host. If the specified directory is not accessible
or does not exist, bsub -Zs cannot write to the default directory
LSB\_SHAREDIR/_cluster\_name_/lsf_cmddir and the job fails.

The -Zs option is not supported for embedded job commands because
LSF is unable to determine the first command to be spooled in an
embedded job command.

Parent topic: Options






**command**



Specifies the command and arguments used for the job submission.





# Synopsis



**bsub** [options] command [arguments]


# Description



The job can be specified by a command line argument command, or
through the standard input if the command is not present on the
command line. The_ command_ can be anything that is provided
to a UNIX Bourne shell (see sh(1)). command is assumed to begin
with the first word that is not part of a bsub option. All
arguments that follow _command_ are provided as the arguments
to the _command_. Use single quotation marks around the
expression if the command or arguments contain special
characters.

The job command can be up to 4094 characters long for UNIX and
Linux or up to 255 characters for Windows. If no job name is
specified with -J, bjobs, bhist and bacct displays the command as
the job name.

If the job is not given on the command line, bsub reads the job
commands from standard input. If the standard input is a
controlling terminal, the user is prompted with bsub&gt; for
the commands of the job. The input is terminated by entering
CTRL-D on a new line. You can submit multiple commands through
standard input.

The commands are executed in the order in which they are given.
bsub options can also be specified in the standard input if the
line begins with #BSUB; for example, #BSUB -x. If an
option is given on both the bsub command line, and in the
standard input, the command line option overrides the option in
the standard input. The user can specify the shell to run the
commands by specifying the shell path name in the first line of
the standard input, such as #!/bin/csh. If the shell is not
given in the first line, the Bourne shell is used. The standard
input facility can be used to spool a user's job script; such as
bsub &lt; script.


# Examples



bsub sleep 100

Submit the UNIX command sleep together with its argument
100 as a job.

bsub my\_script

Submit my\_script as a job. Since my\_script is
specified as a command line argument, the my\_script file is
not spooled. Later changes to the my\_script file before the
job completes may affect this job.

bsub &lt; default_shell_script  


where default_shell_script contains:

sim1.exe  
sim2.exe

The file default_shell_script is spooled, and the commands are
run under the Bourne shell since a shell specification is not
given in the first line of the script.

bsub &lt; csh\_script

where csh\_script contains:

#!/bin/csh  
sim1.exe  
sim2.exe

csh\_script is spooled and the commands are run under
/bin/csh.

bsub -q night &lt; my\_script

where my\_script contains:

#!/bin/sh  
#BSUB -q test  
#BSUB -m "host1 host2" # my default candidate hosts  
#BSUB -f "input &gt; tmp" -f "output &lt;&lt; tmp"  
#BSUB -D 200 -c 10/host1  
#BSUB -t 13:00  
#BSUB -k "dir 5"  
sim1.exe  
sim2.exe

The job is submitted to the night queue instead of
test, because the command line overrides the script.

bsub&gt; sleep 1800  
bsub&gt; my_program  
bsub&gt; CTRL-D

The job commands are entered interactively.

Parent topic: Options






**job\_script**



Specifies the job script for the bsub command to load, parse, and
run from the command line.





# Synopsis



**bsub** [options] job_script


# Description



The **LSB\_BSUB\_PARSE\_SCRIPT** parameter must be set to Y
in the lsf.conf file to specify a job script.

The job script must be an ASCII text file and not a binary file.
The job script does not have to be an executable file because the
contents of the job script are parsed and run as part of the job.

Use the #BSUB imperative (in upper case letters) at the
beginning of each line to specify job submission options in the
script.


# Example



The following script (myscript.sh) uses the #BSUB
imperative to run the myjob1 arg1 and myjob2 arg2 commands with
the bsub -n 2 and -P myproj options:

#!/bin/sh  
#BSUB -n 2  
myjob1 arg1  
myjob2 arg2  
#BSUB -P myproj

Run the following command to use this job script:

bsub myscript.sh

Parent topic: Options






**-h**



Displays a description of the specified category, command option,
or sub-option to stderr and exits.





# Synopsis



**bsub -h**[**elp**] [category ...] [option ...]


# Description



You can abbreviate the -help option to -h.

Run bsub -h (or bsub -help) without a command option or category
name to display the bsub command description.


# Examples



bsub -h io

Displays a description of the io category and the bsub
command options belonging to this category.

bsub -h -app

Displays a detailed description of the bsub -app option.

Parent topic: Options






**-V**



Prints LSF release version to stderr and exits.





# Synopsis



**bsub -V**


# Conflicting Options



Do not use with any other option except -h (bsub -h -V).

Parent topic: Options





**Description**



Submits a job for execution and assigns it a unique numerical job
ID.

Runs the job on a host that satisfies all requirements of the
job, when all conditions on the job, host, queue, application
profile, and cluster are satisfied. If LSF cannot run all jobs
immediately, LSF scheduling policies determine the order of
dispatch. Jobs are started and suspended according to the current
system load.

Sets the user's execution environment for the job, including the
current working directory, file creation mask, and all
environment variables, and sets LSF environment variables before
the job starts.

When a job is run, the command line and stdout/stderr buffers are
stored in the directory /.lsbatch on the execution host. If this
directory is not accessible, /tmp/.lsbtmp is used as the job's
home directory. If the current working directory is under the
home directory on the submission host, then the current working
directory is also set to be the same relative directory under the
home directory on the execution host.

By default, if the current working directory is not accessible on
the execution host, LSF finds a working direction to run the job
in the following order:

1. _$PWD_

2. Strip /tmp_mnt if it is exists in the path

3. Replace the first component with a key in /etc/auto.master and
   try each key

4. Replace the first 2 components with a key in /etc/auto.master
   and try for each key

5. Strip the first level of the path and try the rest (for
   example, if the current working directory is /abc/x/y/z, try
   to change directory to the path /x/y/z)

6. /tmp

If the environment variable **LSB\_EXIT\_IF\_CWD\_NOTEXIST** is set
to Y and the current working directory is not accessible on the
execution host, the job exits with the exit code 2.

If no command is supplied, bsub prompts for the command from the
standard input. On UNIX, the input is terminated by entering
CTRL-D on a new line. On Windows, the input is terminated by
entering CTRL-Z on a new line.

To kill a job that is submitted with bsub, use bkill.

Use bmod to modify jobs that are submitted with bsub. bmod takes
similar options to bsub.

Jobs that are submitted to a chunk job queue with the following
options are not chunked; they are dispatched individually:

*  -I (interactive jobs)

*  -c (jobs with CPU limit greater than 30)

*  -W (jobs with run limit greater than 30 minutes)

To submit jobs from UNIX to display GUIs through Microsoft
Terminal Services on Windows, submit the job with bsub and define
the environment variables **LSF\_LOGON\_DESKTOP=1** and
**LSB\_TSJOB=1** on the UNIX host. Use tssub to submit a
Terminal Services job from Windows hosts. See Using LSF on
Windows for more details.

If the parameter **LSB\_STDOUT\_DIRECT** in lsf.conf is set to Y
or y, and you use the -o or -oo option, the standard output of a
job is written to the file you specify as the job runs. If
**LSB\_STDOUT\_DIRECT** is not set, and you use -o or -oo, the
standard output of a job is written to a temporary file and
copied to the specified file after the job finishes.
**LSB\_STDOUT\_DIRECT** is not supported on Windows.


# Default Behavior



LSF assumes that uniform user names and user ID spaces exist
among all the hosts in the cluster. That is jobs run under on the
execution host the same users account as the job was submitted
with. For situations where nonuniform user names and user ID
spaces exist, account mapping must be used to determine the
account that is used to run a job.

bsub uses the command name as the job name. Quotation marks are
significant.

Options that are related to file names and job spooling
directories support paths that contain up to 4094 characters for
UNIX and Linux, or up to 255 characters for Windows.

Options that are related to command names and job names can
contain up to 4094 characters for UNIX and Linux, or up to 255
characters for Windows.

Options for the following resource usage limits are specified in
KB:

*  Core limit (-C)

*  Memory limit (-M)

*  Stack limit (-S)

*  Swap limit (-v)

Use **LSF\_UNIT\_FOR\_LIMITS** in lsf.conf to specify a larger
unit for the limit (MB, GB, TB, PB, or EB).

If fair share is defined and you belong to multiple user groups,
the job is scheduled under the user group that allows the
quickest dispatch.

The job is not checkpoint-able.

bsub automatically selects an appropriate queue. If you defined a
default queue list by setting **LSB\_DEFAULTQUEUE** environment
variable, the queue is selected from your list. If
**LSB\_DEFAULTQUEUE** is not defined, the queue is selected from
the system default queue list that is specified by the LSF
administrator with the **DEFAULT\_QUEUE** parameter in
lsb.params.

LSF tries to obtain resource requirement information for the job
from the remote task list that is maintained by the load sharing
library. If the job is not listed in the remote task list, the
default resource requirement is to run the job on a host or hosts
that are of the same host type as the submission host.

bsub assumes that only one processor is requested.

bsub does not start a login shell but runs the job file under the
execution environment from which the job was submitted.

The input file for the job is /dev/null (no input).

bsub sends mail to you when the job is done. The default
destination is defined by **LSB\_MAILTO** in lsf.conf. The mail
message includes the job report, the job output (if any), and the
error message (if any).

bsub charges the job to the default project. The default project
is the project that you define by setting the environment
variable **LSB\_DEFAULTPROJECT**. If you do not set
**LSB\_DEFAULTPROJECT**, the default project is the project that
is specified by the LSF administrator with **DEFAULT\_PROJECT**
parameter in lsb.params. If **DEFAULT\_PROJECT** is not defined,
then LSF uses default as the default project name.


# Output



If the job is successfully submitted, bsub displays the job ID
and the queue that the job was submitted to.


# Limitations



When account mapping is used, the command bpeek does not work.
File transfer with the -f option to bsub requires rcp to be
working between the submission and execution hosts. Use the -N
option to request mail, or the -o and -e options to specify an
output file and error file.


# Submitting Jobs with Ssh



Secure Shell (SSH) is a network protocol that provides
confidentiality and integrity of data using a secure channel
between two networked devices.

SSH uses public-key cryptography to authenticate the remote
computer and allow the remote computer to authenticate the user,
if necessary.

SSH is typically used to log into a remote machine and execute
commands, but it also supports tunneling, forwarding arbitrary
TCP ports and X11 connections. SSH uses a client-server protocol.

SSH uses private/public key pairs to log into another host. Users
no longer have to supply a password every time they log on to a
remote host.

SSH is used when running any of the following jobs:

*  An interactive job (bsub -IS | -ISp | ISs)

*  An interactive X-window job with X11 forwarding (bsub -XF)

*  An interactive X-window job, without X11 forwarding (bsub -IX)

*  An externally submitted job (esub)

SSH only supports UNIX for submission and execution hosts. The
display host can be any operating system.

Depending on your requirements for X-Window jobs, you can choose
either bsub -XF (recommended) or bsub -IX. Both options encrypt
the X-Server and X-Clients.

SSH X11 forwarding (bsub -XF) has the following benefits:

*  Any password required can be typed in when needed.

*  Does not require the X-Server host to have the SSH daemon
   installed.

SSH X11 forwarding has the following drawbacks:

*  The user must enable X11 forwarding in the client.

*  Submission and execution hosts must be UNIX hosts.

SSH X11 forwarding has the following dependencies:

*  OpenSSH 3.9p1 and up is supported.

   OpenSSL 0.9.7a and up is supported.

*  You must have SSH correctly installed on all hosts in the
   cluster.

*  You must use an SSH client to log on to the submission host
   from the display host.

*  You must install and run the X-Server program on the display
   host.

SSH X11 forwarding has the following limitations:

*  You cannot run with bsub -K, -IX, or -r.

*  You cannot bmod a job submitted with X11 forwarding.

*  Cannot be used with job arrays, job chunks, or user account
   mapping.

*  Jobs submitted with X11 forwarding cannot be checked or
   modified by esubs.

*  Can only run on UNIX hosts (submission and execution hosts).

SSH interactive X-Window (bsub -IX) has the following benefits:

*  The execution host contacts the X-Server host directly (no
   user steps required).

*  Hosts can be any OS that OpenSSH supports.

SSH interactive X-Window has the following drawbacks:

*  Requires the SSH daemon installed on the X-Server host.

*  Must use private keys with no passwords set.

SSH interactive X-Window has the following dependencies:

*  You must have OpenSSH correctly installed on all hosts in the
   cluster.

*  You must generate public/private key pairs and add the content
   of the public key to the authorized_keys file on remote hosts.
   For more information, refer to your SSH documentation.

*  For X-window jobs, you must set the DISPLAY environment
   variable to _X-serverHost_:0.0, where
   _X-serverHost_ is the name of the X-window server. Ensure
   that the X-server can access itself. Run, for example, xhost
   +localhost.

SSH interactive X-Window has the following limitations:

*  Cannot be used with job arrays or job chunks.

*  Private user keys must have no password set.

*  You cannot run with -K , -r, or -XF.


# See Also



bjobs, bkill, bqueues, bhosts, bmgroup, bmod, bchkpnt, brestart,
bgadd, bgdel, bjgroup, sh, getrlimit, sbrk, libckpt.a,
lsb.users, lsb.queues, lsb.params, lsb.hosts, lsb.serviceclasses,
mbatchd

Parent topic: bsub
