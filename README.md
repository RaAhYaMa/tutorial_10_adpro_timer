## Experiment 1.2: Understanding how it works.

In this experiment, we added a `println!` statement after spawning a task to print before and after waiting on a timer. We also added a sentence to explain what happened.

The output of the program is:
```
Yasin's Computer: hey hey
Yasin's Computer: howdy!
Yasin's Computer: done!
```

The result is as such because the `spawner.spawn` function only schedules the task to be run, but does not actually run it. The task is only executed when the `executor.run` function is called. Therefore, the `println!` statement after `spawner.spawn` is executed before the task is run, and the `println!` statements inside the task are executed after the task is run.



Here is the README.md file:

## Experiment 1.3: Multiple Spawn and removing drop

### Multiple Spawn

We will replicate the spawn statement multiple times to see how it affects the output.

```rust
fn main() {
    let (executor, spawner) = new_executor_and_spawner();

    // Spawn a task to print before and after waiting on a timer.
    spawner.spawn(async {
        println!("Yasin's Computer: howdy!");
        // Wait for our timer future to complete after two seconds.
        TimerFuture::new(Duration::new(2, 0)).await;
        println!("Yasin's Computer: done!");
    });

    spawner.spawn(async {
        println!("Yasin's Computer: howdy2!");
        // Wait for our timer future to complete after two seconds.
        TimerFuture::new(Duration::new(2, 0)).await;
        println!("Yasin's Computer: done2!");
    });

    spawner.spawn(async {
        println!("Yasin's Computer: howdy3!");
        // Wait for our timer future to complete after two seconds.
        TimerFuture::new(Duration::new(2, 0)).await;
        println!("Yasin's Computer: done3!");
    });

    println!("Yasin's Computer: hey hey");
    // Drop the spawner so that our executor knows it is finished and won't
    // receive more incoming tasks to run.
    drop(spawner);

    // Run the executor until the task queue is empty.
    // This will print "howdy!", pause, and then print "done!".
    executor.run();
}
```

The output of the program is:
```
Yasin's Computer: hey hey
Yasin's Computer: howdy!
Yasin's Computer: howdy2!
Yasin's Computer: howdy3!
Yasin's Computer: done3!
Yasin's Computer: done!
Yasin's Computer: done2!
```

### Removing drop(spawner)

We will remove the `drop(spawner)` statement to see how it affects the output.

```rust
fn main() {
    let (executor, spawner) = new_executor_and_spawner();

    // Spawn a task to print before and after waiting on a timer.
    spawner.spawn(async {
        println!("Yasin's Computer: howdy!");
        // Wait for our timer future to complete after two seconds.
        TimerFuture::new(Duration::new(2, 0)).await;
        println!("Yasin's Computer: done!");
    });

    spawner.spawn(async {
        println!("Yasin's Computer: howdy2!");
        // Wait for our timer future to complete after two seconds.
        TimerFuture::new(Duration::new(2, 0)).await;
        println!("Yasin's Computer: done2!");
    });

    spawner.spawn(async {
        println!("Yasin's Computer: howdy3!");
        // Wait for our timer future to complete after two seconds.
        TimerFuture::new(Duration::new(2, 0)).await;
        println!("Yasin's Computer: done3!");
    });

    println!("Yasin's Computer: hey hey");
    // Drop the spawner so that our executor knows it is finished and won't
    // receive more incoming tasks to run.
    // drop(spawner);

    // Run the executor until the task queue is empty.
    // This will print "howdy!", pause, and then print "done!".
    executor.run();
}
```

The output of the program is:
```
Yasin's Computer: hey hey
Yasin's Computer: howdy!
Yasin's Computer: howdy2!
Yasin's Computer: howdy3!
Yasin's Computer: done!
Yasin's Computer: done2!
Yasin's Computer: done3!
^C # Must be stop using CTRL + C
```

### Observations

When we run the code with multiple spawn statements, we observe that both tasks are executed concurrently, and the output is interleaved.

When we remove the `drop(spawner)` statement, we observe that the executor continues to run indefinitely, waiting for new tasks to be spawned. This is because the executor is not notified that there are no more tasks to be executed.

### Conclusion

In this experiment, we have demonstrated the effect of spawning multiple tasks and the role of the spawner, executor, and drop statement in asynchronous programming. We have shown that the `drop(spawner)` statement is necessary to notify the executor that there are no more tasks to be executed, and that removing it causes the executor to run indefinitely.