<details>
<summary><strong>Concurrency</strong></summary>
  
Synchronization always has some cost, but an unsynchronized data race is incorrect. 

`RWMutex` helps only when reads are frequent, sufficiently long, and concurrent.

For short critical sections or frequent writes, a plain `sync.Mutex` can be faster because it has less bookkeeping and contention.

Other approaches include:

- sync.Mutex: usually the best default for shared mutable state.
- Channels: transfer ownership or coordinate work instead of sharing memory.
- sync/atomic: efficient for simple counters, flags, and carefully designed immutable snapshots.
- sync.Map: useful for specific concurrent-map workloads, not a general replacement for map plus a mutex.
- Sharding: divide a map or other resource across multiple locks to reduce contention.
- Immutable/copy-on-write data: readers need little or no locking; best when writes are rare.
- Goroutine ownership: one goroutine owns the state, while others send requests through channels.
- Avoid sharing: keep data local to each goroutine and combine results afterward.

A practical rule:
- Start with `sync.Mutex`.
- Benchmark before replacing it with RWMutex.
- Reduce the amount of work done while holding the lock.
- Use channels for coordination or ownership transfer—not merely to avoid mutexes.
- Run `go test -race ./...` to detect data races.
</details>
