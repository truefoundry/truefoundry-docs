# Customize your job with advanced options

### Configure retry limit

You can specify the maximum number of attempts to run a job before it is marked as failed.

// TODO: Add Python / YAML code

### Configure Job Time Limit

You can specify (in seconds) the maximum amount of time for a job to run, whether it has failed or not. This will take precedence over the Retry Limit.

For example, if you set a Retry Limit of 6 and a Time Limit of 480, the job will terminate after 8 minutes regardless of how many times it attempted to run.

// TODO: Add Python / YAML code

### Set resources limits

You can configure the CPU, memory and GPU resources to be allocated to each job. To understand the resources configuration in more details, please read the docs below. 

// TODO: Add a page on the resources section

### Set job history limit

You can set how much history of jobs to retain. We have options to decide how many of the successful and the failed jobs to keep in history. This helps keep track of the previous completed / failed jobs for debugging purposes. 

successful_jobs_history_limit