
This project is to help understand how Re-Reference Inverval Prediction (RRIP) cache replacement policy works. And implement the Static RRIP Hit Priority (SRRIP-HP) policy for
shared L3 caches in zsim; and compare its performance to other policies.

RRIP finds a victim block that is not recently used in a set for cache replacement. When replace a new block in the cache, its recency value is not set to near-immediate re-reference interval (Most recently used) or distant re-reference interval (Least recently used). Instead, it is set to a long re-reference interval between the two extremes. 

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

NOTE: SRRIP policy is created in rrip_repl file -> zsim/src/rrip_repl
