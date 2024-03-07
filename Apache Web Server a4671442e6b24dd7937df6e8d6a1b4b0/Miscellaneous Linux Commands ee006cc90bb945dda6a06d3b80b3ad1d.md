# Miscellaneous Linux Commands

- USE of grep
    
    Some grep options
    
    `grep ^abc` when we write this, then we mean that in the beginning of line `abc` string should be present
    
    `grep -B1 ^abc` **B1** here just means that show us 1 line **before** our result 
    
    Similarly, **A1** means show 1 line **after** our result
    
- USE of head
    
    it is used in order to get desired number of lines as our output
    
    For eg: `elinks --dump [http://172.24.0.1](http://172.24.0.1) | head -5`
    
    `head` will only show first 5 lines of output.