# ZIO Schedule cheatsheet

## Common cases

Case | Schedule 
----------|---------
Repeat 3 times, with unbounded exponential backoff | `Schedule.exponential(100.millis) && Schedule.recurs(3)`
Repeat infinitely, with exponential backoff. Each backoff is bounded by 5 seconds. | ```Schedule.exponential(1.second) \|\| Schedule.spaced(5.seconds)``` `Schedule.exponential(1.second) \|\| Schedule.fixed(5.seconds)`
Repeat within 30 seconds, with exponentional backoff [1..5] seconds | `((Schedule.exponential(1.second) \|\| Schedule.spaced(5.seconds)) >>> Schedule.elapsed).whileOutput(_ < 30.seconds)` or `zio.repeat(Schedule.exponential(1.second) \|\| Schedule.spaced(5.seconds)).timeout(30.seconds)`



## Common mistakes
Mistake | Schedule 
----------|---------
Repeat 3 times with zero backoff, and then infinitely with unbounded exponentional backoff | `Schedule.exponential(100.millis) \|\| Schedule.recurs(3)`
Repeat infininely with unbounded exponentional backoff | `Schedule.exponential(1.second) \|\| Schedule.duration(5.seconds)`
Repeat infininely with unbounded exponentional backoff, starting from 5 seconds | `Schedule.exponential(1.second) && Schedule.fixed(5.seconds)`
Repeat 1 time, with 5 second backoff | `Schedule.exponential(1.second) && Schedule.duration(5.seconds)`
