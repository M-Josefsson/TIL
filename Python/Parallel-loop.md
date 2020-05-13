Simple way to parallelize a for-loop using joblib.

```
from joblib import Parallel, delayed
import multiprocessing

num_cores = multiprocessing.cpu_count()
result = Parallel(n_jobs=num_cores)(delayed(my_function)(x) for x in x_to_be_looped)
```
