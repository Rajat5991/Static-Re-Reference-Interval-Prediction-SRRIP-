# Static-Re-Reference-Interval-Prediction-SRRIP

This project is to help understand how Re-Reference Inverval Prediction (RRIP) cache replacement policy works. And implement the Static RRIP Hit Priority (SRRIP-HP) policy for
shared L3 caches in zsim; and compare its performance to other policies.

RRIP finds a victim block that is not recently used in a set for cache replacement. When replace a new block in the cache, its recency value is not set to near-immediate re-reference interval (Most
recently used) or distant re-reference interval (Least recently used). Instead, it is set to a long re-reference interval between the two extremes. 

# LRU - Least Recently Used

LRU is a popular cache replacement policy that removes the least recently used items from the cache when it is full. This policy assumes that the data that has not been accessed for the longest time is less likely to be accessed again soon.

# LFU - Least Frequently Used

LFU is a cache replacement policy that removes the least frequently used items from the cache when it is full. It relies on a counter associated with each blocks in the cache, tracking the number of times the block has been accessed.

# NRU - Not Recently Used

With only one bit of information, NRU allows two re-reference interval predictions: near-immediate re-reference and distant rereference. An nru-bit value of ‘0’ implies that a block was recently
used and the block is predicted to be re-referenced in the nearimmediate future. An nru-bit value of ‘1’ implies that the block was not recently used and the block is predicted to be re-referenced in the distant future. On cache fills, NRU always predicts that the missing block will have a near-immediate re-reference.  On a cache miss, NRU selects the victim cache block whose predicted re-reference is in the distant future, i.e., a block whose nru-bit is ‘1’.
