1. stack object alloacation - arrays
2. devirtualization 
3. bounds checking
   извращения с лог2 - возвращает не больше Х, можно убрать баунд чек
   no bounds check for checked length in case statements
   cloning for bounds checks
4. cloning 
    for spans
    cloning with PGO with interfaced collactions
5. More inlining (try/c/f, generics, increased inlining budget, local variables tracking, but do not forget about instruction cache)
6. Constant folding
      JIT can fold constants. for example after inlining "constant methods"  static string GetString1() => "Hello"; or proccessor count
7. Code layout - explains what is code layout and that actual algorithms were improved
8. GC Write Barriers
    GC tracks assignments to older gens fields so they are visited when gen0 is collected. Some such checks are eliminated for stack only stuff (ref struct ) 
    return buffers - used for large returns which do not fit return registers, could be on heap - GC WB check, now removed. ALso some arm-specific GC WB optimizations (multiple WB variants)
9. Misc 
    covatiance checks for array - make arrays of sealed types
    More strength reduction.
    return someCondition ? true : false
    Bit tests.
    Better range comparison. When comparing a ulong expression against uint.MaxValue, rather than being emitted as a comparison can be handled more efficiently as a shift.
10. Threadpool
     - SOme dirty magic with TP queues (local, global) which fixes blocking
     - DOTNET_ThreadPool_CpuUtilizationIntervalMs
     - removed lock contention on creation of nw threadpool threads
     - new Volatile methods for bitoebov
     - reduced allocations in Task.WhenAll, no extra work for single task WhenAll - the task itself is returned
     - channels switched from arrays to LLs
11. Reflection 
    - UnsafeAccessorType Http+Crypto 
    - Reduces some overheads and allocations as part of parsing and rendering TypeNames.
12. Types
     - datetime types (some funny shit in PRs)
     - Guid Version + IUtf8SpanParsable
     - some decimal Uint128 BigInteger improvements 
     - System.Numerics.Tensors extended + optimized + compound operator (= + overload for +=, removes allocation for tensor)
13. Collections
    - dynamic PGO, escape analysis, and stack allocation => no more allocation of enumerator for lists and arrays in many cases when working via interface
      Enumerator is Intrinsic
      + funny pgo stuff for different sized lists
    - `Stack<T>` enumerator [rewritten](https://github.com/dotnet/runtime/pull/117328) ++ perf and [`Queue<T>`](https://github.com/dotnet/runtime/pull/117341)
    - [ConcurrentDictionary](https://github.com/dotnet/runtime/pull/116949) -  irreducible loop -> reducible loop
14. LINQ
    - `Contains` skips work for `OrderBy`, `Distinct`, and `Reverse` (like `OrderBy` + `First...`) - [almost 30 specialized implementations of `Contains` across the various iterator specializations](https://github.com/dotnet/runtime/pull/112684)
    - `Shuffle` + new method with the same designs (`Count`, `First`, ...) + `Take` ["reservoir sampling."](https://en.wikipedia.org/wiki/Reservoir_sampling) `+ Take.Contains` (hypergeometric distribution)
    - `LeftJoin` and `RightJoin` (better perf than alternatives)
15.    
