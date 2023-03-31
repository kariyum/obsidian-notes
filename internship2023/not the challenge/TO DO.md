# Notes
1. Redis is single threaded lol
2. Starting with Cache-Aside and FIFO eviction strategy + LFU/LRU to compare
3. Using keep-alive connection for a faster simulation

# Questions
1. should we use UDP / TCP / WEBSOCKET between cache and server ?

# What is done
1. Implemented OPR, results are good

# TO DO:
1. some changes in the security service, in the cookie validation step, check note. ![[internship2023/not the challenge/security service/Notes#future updates]]
2. event service: implement function that checks the cache just like validateCookie function, if the data is not in the cache the application procceeds to a normal behaviour. respondFromCache function 