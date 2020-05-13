Here are the shell scripts used to run many small serial jobs in parallel. The master script is sent to sbatch, which then calls the slave script once per serial job. 

Master.sh
```
#!/bin/sh
# requesting the number of nodes needed
#SBATCH -N 1
#SBATCH --tasks-per-node=20
#
# job time, change for what your job farm requires
#SBATCH -t 24:00:00
#
#SBATCH --mail-user=martin.josefsson@ftf.lth.se
#SBATCH --mail-type=END
#
# job name and output file names
#SBATCH -J job
#SBATCH -o job_%j.out
#SBATCH -e job_%j.out
cat $0

# set the number of jobs - change for your requirement
export NB_of_jobs=20

# Loop over the job number

for ((i=1; i<=$NB_of_jobs; i++))
do
    srun -Q --exclusive -n 1 -N 1 slave.sh $i &> worker_${SLURM_JOB_ID}_${i} &
    sleep 1
done

# keep the wait statement, it is important!

wait
```

slave.sh

```

#!/bin/sh
# document this script to stdout (assumes redirection from caller)
cat $0

# receive my worker number
export WRK_NB=$1

# create a variable to address the "job directory"
export JOB_DIR=$SLURM_SUBMIT_DIR/job_${WRK_NB}

# now copy the input data and program from there
cd $JOB_DIR

# load modules

module load GCC/4.9.3-binutils-2.25
module load GSL/2.1

# run the program

