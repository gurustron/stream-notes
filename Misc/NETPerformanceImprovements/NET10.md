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
     - IPAddress and IPNetwork IUtf8SpanParsable (networking) + IsValid (instead of Tryparse with alloc)
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
    - `UseSizeOptimizedLinq` setting for NativeAOT - size vs perfromance (different versions). Some of the more impactful specializations where the throughput gains significantly outweigh the relatively-minimal size cost brought to NativeAOT anyway
15. Frozen Collections
   - `FrozenDictionary` - specializations for size `int` or smaller + enums. Handles dense collections.  
   - `FrozenDictionary` and `FrozenSet` - GVM (generic method parameter (the alternate key type)) Rather than the lookup operation itself being a generic virtual method, the PR introduces a separate generic virtual method that retrieves a delegate for performing the lookup; the retrieval of that delegate still incurs GVM penalties, but once the delegate is retrieved, it can be cached, and invoking it does not incur said overheads. 
16. BitArray
   - `CollectionsMarshal.AsBytes(BitArray)` method that returns a `Span<byte>` directly referencing the BitArray‘s underlying storage.
17. Misc Collections
   - List.InsertRange now uses internal `GrowForInsertion` (introduced in .NET 9) which copies parts before and after the insertion to prevent double copying
   - ConcurrentDictionary remembers originally requested size (capacity) on clearing TODO: check out how effective are knobs on the dict with concurrency levels and grow locks
   - Dictionary with string keys and constant keys lookups works faster. https://github.com/dotnet/runtime/pull/117427/files
   - `OrderedDictionary` `TryGetValue`/`TryAdd` return `out index` which can be used in `SetAt`
   - `InlineArray`
      - implicitly convertable to `Span<T>`
      - `InlineArray2<T>`, `InlineArray3<T>` to prevent code bloat (previosly generated in-place for every assembly)    
    
18. I/O
   - `FileSystemWatcher` - fixed leak on Windows (disposing why in use) + improved interop 
   - `BufferedStream.WriteByte` was flushing instead of just writing which could be costly in some underlying streams (`DeflateStream` for example)
   - `zlib-ng` used by DeflateStream/GZipStream/ZLibStream (managed wrappers) is updated to 2.2.5
   - `ZipArchive`
      - not reling on BinaryReader and BinaryWriter, avoiding their underlying buffer allocations and having more fine-grained control over how and when exactly data is encoded/decoded and written/read
      - A ZipArchive update used to be “rewrite the world”: it loaded every entry’s data into memory and rewrote all the file headers, all entry data, and the “central directory” (what the zip format calls its catalog of all the entries in the archive). A large archive would have proportionally large allocation. This PR introduces change tracking plus ordering of entries so that only the portion of the file from the first actually affected entry (or one whose variable‑length metadata/data changed) is rewritten, rather than always rewriting the whole thing.Faster + reduce alloc
      - ZipArchive and ZipFile also gain async APIs
   - StreamReader.EndOfStream is prop perfroming IO to determine if it is the end, which is blocking etc. (`while (!reader.EndOfStream)` ... should be `while (await reader.ReadLineAsync() is string line)`)
19. Networking
   - Uri was limited to 65k (uri can contain data itself for example images `data:application/octet-stream;base64`) due to using ushort for part delimeters, now those are ints, Uri implements path compression (`Console.WriteLine(new Uri("http://test/hello/../hello/../hello"));`) which was O(N^2) (not a problem for bounded URIs), now [O(N)](https://github.com/dotnet/runtime/pull/117820) which removed the attack vector
   - `GetStringAsync` and `GetByteArrayAsync` became faster/less allocatey by using internal `LimitArrayPoolWriteStream` [PR](https://github.com/dotnet/runtime/pull/109642)
   - Native AOT has a switch to remove HTTP/3 support from HttpClient (if you are not using it)
20. Regex
   - backtracking vs non-backtracking atomic groups. .NET regex optimizer can add (yep, really, .NET 10 for example does [`@"\d+\D"`->`@"(?>\d+)\D")`](https://github.com/dotnet/runtime/blob/d3dffb9eb5f1ae5368d47007223017028e5e1223/src/libraries/System.Text.RegularExpressions/tests/UnitTests/RegexReductionTests.cs#L421), or previous versions `a|e|i|o|u` -> `[aeiou]`) atomic groups when no observable effect.
       20,000 regular expression patterns in database of real-world regexes sourced from nuget. .NET 10 over 70% of those patterns have at least one construct upgraded to be atomic
   - Remove capturing groups (make them non-capturing) in the look-arounds, look-arounds for zero-width patterns (`(?=$)` -> `$`)
   - Small intresting improvement for patterns like `\b\w+\b` -> "full checks" `Utilities.IsBoundary` -> "half checks"  `Utilities.IsPreWordCharBoundary/IsPostWordCharBoundary`
   - Handling for more "everytning" patterns (`\n|.`)
   - More rewrites  (`\r\n|\r` -> `\r(?:\n|)` -> `\r\n?` -> `\r(?>\n?)`) , ~20% perfr improvement for patterns like `@"ab|a"`
   - `TryFindNextPossibleStartingPosition` -> `TryMatchAtCurrentPosition`, formar now considers lookarounds   
21. `MemoryExtensions` now
   - not constrained `IEquatable<T>`
   - Can accept non-default comparer
22. as
23. assa
