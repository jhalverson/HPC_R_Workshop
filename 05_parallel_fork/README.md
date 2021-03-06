# parallel library

R has built in multicore support under Linux that doesn't require MPI. If
your problem can use a more limited number of cores, it offers an easy
(very easy) way to use the number of cores you have using a suite of functions
that are part of the built-in `parallel` library, which builds on two
older packages (`multicore` and `snow`). `snow` still exists and has
MPI and sockets cluster implementations (though the former has been
problematic with OpenMPI 2.0 so I do not recommend it).

`parallel` works by forking process from the parent, so the SLURM script
will look different. It will specify `-N 1 -n 1` (one node, one process)
and then add `-c 5`, which says assign 5 CPUs to that process.

This can only run across one node, but it is very quick, very easy, and it 
can accomodate parallelizing operations across large dataframes readily.

## The example

This is an adaptation of a standard parallelize and reduce that has (admittedly)
limited application. However, if you can use the apply family of functions, you
can do the same with their parallel breathren. It also demonstrates the
`--mem=50g` flag in the example script. The matrix we make is quite large, and 
CPUs come with a default allocation (5 GB x 5) that isn't quite enough, so we 
just ask for more.

Since this task is isolated to one node and one shared parent, we can merrily
just increase the memory up to the node max (though that will slow down how it takes
the job to start as it waits for open resources).
