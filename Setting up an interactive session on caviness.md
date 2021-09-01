In one shell:
- ```ssh user_name@caviness.hpc.udel.edu```
- ```workgroup -g dsi```
```screen```
```salloc --partition=devel --cpus-per-task=2 OR\
               salloc --partition=_workgroup_ --gres=gpu --time=1:00:00```
              
```jupyter notebook --no-browser --port=18889```

In a second shell:
```ssh user_name@caviness.hpc.udel.edu```
```workgroup -g dsi```
```screen```
```ssh -N -f -L localhost:18880:localhost:18889 hostname```

In a third shell:
```ssh -N -f -L localhost:8703:localhost:18880 user_name@login01.caviness.hpc.udel.edu```

Note that the login01 in the line above should be the same login node as the one in the second shell.

Copy in your browser: localhost:8703
