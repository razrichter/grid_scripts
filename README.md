# Grid Scripts

## qrun

qrun is a simplified method for submitting a set of related commands to the grid, inspired by [gnu parallel](https://www.gnu.org/software/parallel/man.html). Using a command template and a tab-delimited input you can submit a batch job to run the script for each line of the input file.

For Example:

        echo >>_options
        Alex   sheep     great
        Dan    cows      yummy
        Bill   cockroaches    gross
        _options | qrun -P 1010 -c 'echo "{1} says {2} are {3}'

Will start three grid jobs, with the output:

    Alex says sheep are great
    Dan says cows are yummy
    Bill says cockroaches are gross

Run `qrun --man` for full documentation.
 If you want to see what would be run, use `qrun --dry_run`. If you want details on what is run (with output to STDERR) use `qrun -v`

## qstatus

qstatus is a wrapper around SGE qstat to give more detailed information about running and queued jobs. Beyond having nicer output, it differs from qstat -g by also showing queued jobs and by summing by slot(node) rather by job, and by showing entire job names, instead of truncating them.

### Usage

See Modes Below

        qstatus queues

        qstatus users [-q <queue>]

        qstatus jobs [-q <queue>] [-a|-u <user>]

### Modes

#### Queues

Summary of jobs per queue

        $ qstatus queues
        Queue                Load Total Available Disabled Running Error Queued
        default.q            0.94   704         0        0     704     2 125865
        fast.q               0.01    48        48        0       0     0      0
        himem.q              0.11   196       165        0      31     0      0
        interactive.q        0.03    40        23        0      17     0      0
        medium.q             0.01   816       814        0       2     0      0

#### Users

Summary of jobs by user by queue, optionally filtered by queue

        $ qstatus users -q default.q
        User           Queue           Running Pending Error
        razrichter     default.q           703  125858     0
        inger          default.q             0       3     0
        bwayne         default.q             0       4     0
        estark         default.q             0       0     2
        sauron         default.q             1       0     0

#### Jobs

Summary of jobs by job-id, optionally filtered by user and queue

By default, filtered for only your jobs

        $ qstatus jobs -u razrichter
        Job-ID   Owner      Queue           r Job-Name
        5264368  razrichter    himem.q      8 mira e_coli
        5264370  razrichter    normal.q     1 bwa

## qwait

qwait simply waits for a job to complete, optionally printing "." every 60 seconds.

### Usage

        qwait [-q] <job_id>

#### Options

* -q -- suppress progress dots
* <job_id> -- the number (or name) of the job you want to wait for.
