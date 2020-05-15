Basic shell script for serial jobs.

```shell
#!/bin/bash
#
# job time, change for what your job requires
#SBATCH -t 00:10:00
#
#SBATCH --mail-user=martin.josefsson@ftf.lth.se
#SBATCH --mail-type=END
#
# job name
#SBATCH -J job_name
#
# filenames stdout and stderr - customise, include %j
#SBATCH -o job_%j.out
#SBATCH -e job_%j.err

# write this script to stdout-file - useful for scripting errors
cat $0

# load the modules required for you program - customise for your program
module load XX

# run the program
# customise for your program name and add arguments if required
./my_program
```
