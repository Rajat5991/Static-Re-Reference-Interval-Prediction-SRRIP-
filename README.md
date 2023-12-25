
This project is to help understand how Re-Reference Inverval Prediction (RRIP) cache replacement policy works. And implement the Static RRIP Hit Priority (SRRIP-HP) policy for
shared L3 caches in zsim; and compare its performance to other policies.

RRIP finds a victim block that is not recently used in a set for cache replacement. When replace a new block in the cache, its recency value is not set to near-immediate re-reference interval (Most recently used) or distant re-reference interval (Least recently used). Instead, it is set to a long re-reference interval between the two extremes. 

Predicting that every cache insertion will be re-referenced very soon hurts cache performance in mixed access patterns. This happens because scan blocks use up cache space without providing any cache hits. On the other hand, always predicting a distant re-reference interval significantly reduces cache performance for patterns that mostly involve near-immediate re-references. Without external information on the re-reference interval for each missing cache block, the Not Recently Used (NRU) approach cannot identify and keep non-scan blocks in mixed access patterns.

Paper Link: https://dl.acm.org/doi/pdf/10.1145/1815961.1815971

# LRU - Least Recently Used

LRU is a popular cache replacement policy that removes the least recently used items from the cache when it is full. This policy assumes that the data that has not been accessed for the longest time is less likely to be accessed again soon.

# LFU - Least Frequently Used

LFU is a cache replacement policy that removes the least frequently used items from the cache when it is full. It relies on a counter associated with each blocks in the cache, tracking the number of times the block has been accessed.

# NRU - Not Recently Used

With only one bit of information, NRU allows two re-reference interval predictions: near-immediate re-reference and distant rereference. An nru-bit value of ‘0’ implies that a block was recently
used and the block is predicted to be re-referenced in the nearimmediate future. An nru-bit value of ‘1’ implies that the block was not recently used and the block is predicted to be re-referenced in the distant future. On cache fills, NRU always predicts that the missing block will have a near-immediate re-reference.  On a cache miss, NRU selects the victim cache block whose predicted re-reference is in the distant future, i.e., a block whose nru-bit is ‘1’.

# SRRIP - Static  RE-REFERENCE INTERVAL PREDICTION

Static Re-reference Interval Prediction (SRRIP).SRRIP uses M-bits per cache block to store one of 2M possible Rereference Prediction Values (RRPV). RRIP dynamically learns rereference information for each block in the cache access pattern. Like NRU, an RRPV of zero implies that a cache block is predicted to be re-referenced in the near-immediate future while RRPV of saturation
(i.e., 2M–1) implies that a cache block is predicted to be re-referenced in the distant future. Quantitatively, RRIP predicts that blocks with small RRPVs are re-referenced sooner than blocks with large RRPVs. When M=1, RRIP is identical to the NRU replacement policy. When M>1, RRIP enables intermediate re-reference intervals that are greater than a near-immediate re-reference interval but less than a distant re-reference interval. 

<img width="650" alt="image" src="https://github.com/Rajat5991/Static-Re-Reference-Interval-Prediction-SRRIP-/assets/154459536/c31a2904-0ffa-490e-a089-35092b9cc662">


NOTE: SRRIP policy is created in rrip_repl file -> zsim/src/rrip_repl

#  System Requirement

Linux operating system is required in order to use the zsim. Do not use Cygwin, Mac OS X is not supported.

# Environemnt setup

Everytime you want to build or run zsim, you need to setup the environment variables first.

```
$ source setup_env
```

# Compile zsim

```
$ cd zsim
$ scons -j4
```

# Launch a test to run

```
./build/opt/zsim tests/simple.cfg
```

# Procedure
Implement a SRRIP-HP cache replacement policy in zsim. Run simulations to compare the performance to other policies using SPEC CPU2006 on single-core and PARSEC benchmarks on multicore
processor.

   Run the following benchmarks using ZSim
   
   SPEC CPU2006 Benchmarks (Single-threaded)
   
    Integer bzip2, gcc, mcf, hmmer, sjeng, libquantum, xalancbmk, Floating Point milc, cactusADM, leslie3d, namd, soplex, calculix, lbm
    
   PARSEC Benchmarks (Multi-threaded)
   
    blackschoels, bodytrack, canneal, dedup, fluidanimate, freqmine, streamcluster, swaptions, x264
    
Run an representative simpoint of each SPEC CPU2006 benchmark for 100 million
instructions. For PARSEC benchmarks, simulations will run the whole parallel phase.
1. Use runscript to run the benchmarks
Note: The config files and runscript must be in the /zsim.

   $ ./runscript suite benchmark repl policy

Example: $ ./runscript SPEC bzip2 LRU

3. Check the results in outputs directory (zsim.out). In <benchmark>.log we may see
one of the following errors.

# Result Analysis

Comparing IPC and MPKI (misses per thousand instructions) across benchmarks reveals nuanced differences between LRU and SRRIP cache replacement policies. In Parsec benchmarks, like bodytrack and fluidanimate, both policies show similar results. In Spec CPU2006 benchmarks (bzip2, gcc, hmmer), performance is largely similar under both policies, with minor variations in IPC and MPKI values. Notable differences surface in soplex and sjeng, where SRRIP slightly improves IPC and lowers MPKI. Conversely, milc and lbm slightly favor the LRU policy. 

The comparison of cycle counts among LRU, LFU, and SRRIP cache replacement policies across various benchmarks reveals insights into the efficiency of these policies. In Parsec benchmarks, bodytrack and fluidanimate show similar cycle counts under all policies, emphasizing consistency in performance. Notably, the canneal benchmark exhibits variability in cycle counts, with LFU generally performing better.
In Spec CPU2006 benchmarks, bzip2, gcc, and hmmer demonstrate comparable cycle counts across all policies, indicating similar performance levels. Noteworthy variations are observed in soplex and sjeng, where SRRIP generally outperforms LRU and LFU in terms of cycle efficiency. The canneal benchmark presents variations in cycle counts, with LFU exhibiting better performance.

<img width="619" alt="image" src="https://github.com/Rajat5991/Static-Re-Reference-Interval-Prediction-SRRIP-/assets/154459536/773039e2-8606-4190-b10d-a04644dc2453">

<img width="625" alt="image" src="https://github.com/Rajat5991/Static-Re-Reference-Interval-Prediction-SRRIP-/assets/154459536/bc134755-ab8e-4c57-bdcc-40153fdb547b">

<img width="620" alt="image" src="https://github.com/Rajat5991/Static-Re-Reference-Interval-Prediction-SRRIP-/assets/154459536/1949b523-b719-431f-9d5b-2c45857cf889">

<img width="523" alt="image" src="https://github.com/Rajat5991/Static-Re-Reference-Interval-Prediction-SRRIP-/assets/154459536/b8a167b4-9464-4f10-95bc-0ca9fcf792b4">




