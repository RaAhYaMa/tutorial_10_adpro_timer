## Experiment 1.2: Understanding how it works.

In this experiment, we added a `println!` statement after spawning a task to print before and after waiting on a timer. We also added a sentence to explain what happened.

The output of the program is:
```
Yasin's Computer: hey hey
Yasin's Computer: howdy!
Yasin's Computer: done!
```

The result is as such because the `spawner.spawn` function only schedules the task to be run, but does not actually run it. The task is only executed when the `executor.run` function is called. Therefore, the `println!` statement after `spawner.spawn` is executed before the task is run, and the `println!` statements inside the task are executed after the task is run.
