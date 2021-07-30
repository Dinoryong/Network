# Parallelism

------

> [toc]



## 1. Amdahl's law





## 2, Pipelining

- batches

- 



## 3. Concurrency

- conflict

- e.g) the line at a bank, voicemail, assembly line(X)

- sol. Lamport's bakery algorithm



### Summary

- The most common kind of parallelism is splitting up a problem and assigning it to different workers.
- If you can't use parallelism on every part of a problem, adding more and more workers may not help you very much.
- When you need to work on different problems at the same time, it can be useful to use pipeline parallelism.
- When different tasks need exclusive access to a single shared resource in an unpredictable order, you are dealing with concurrency.
- Concurrency and parallelism show up in many of the same places, but they are different ideas.



