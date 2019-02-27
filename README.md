# utl-reducing-two-table-join-from-hours-to-under-three-minutes-SQL-index
Reducing two table join from hours to under three minutes
    Reducing two table join from hours to under three minutes     
    
    github                                                                                                                   
    https://tinyurl.com/y58585ao                                                                                             
    https://github.com/rogerjdeangelis/utl-reducing-two-table-join-from-hours-to-under-three-minutes-SQL-index               
                                                                                                                         
    SAS Forum                                                                                                                
    https://tinyurl.com/y2coc97v                                                                                             
    https://communities.sas.com/t5/SAS-Programming/Macro-length-of-the-value-of-the-macro-variable-exceeds-maximum/m-p/539042
                                                                                                                         

                                                                                                    
    Two tables of 200,00,000 observations                                                           
                                                                                                    
       Table Have1 has 19,020 distinct keys                                                         
       Table Have1 has  1,902 distinct keys                                                         
                                                                                                    
     RULES join tables                                                                              
                                                                                                    
      select                                                                                        
        l.*                                                                                         
      from                                                                                          
        have1 as l, have2 as r                                                                      
      where                                                                                         
       l.key = r.key                                                                                
                                                                                                    
    Fortunately it looks like you do not have very large data,                                      
    Data is quit small both tables are only around 2gb.                                             
                                                                                                    
    There are faster ways to do this but this took less than 3 minutes                              
                                                                                                    
    Do not see a need to parallelize this problem.                                                  
                                                                                                    
    *_                   _                                                                          
    (_)_ __  _ __  _   _| |_                                                                        
    | | '_ \| '_ \| | | | __|                                                                       
    | | | | | |_) | |_| | |_                                                                        
    |_|_| |_| .__/ \__,_|\__|                                                                       
            |_|                                                                                     
    ;                                                                                               
                                                                                                    
    /* I paaded with 1s to get 19,020 distinct values */                                            
                                                                                                    
    WORK.HAVE1 total obs=200,000,019                                                                
                                                                                                    
     KEY        NAME     SEX    AGE    HEIGHT    WEIGHT                                             
                                                                                                    
      1        Alfred     M      14      69       112.5                                             
      1        Alfred     M      14      69       112.5                                             
      1        Alfred     M      14      69       112.5                                             
      1        Alfred     M      14      69       112.5                                             
     ...                                                                                            
    100000000  Alfred     M      14      69       112.5                                             
    100000001  Alfred     M      14      69       112.5                                             
    100000002  Alfred     M      14      69       112.5                                             
    ...,                                                                                            
      1        Alfred     M      14      69       112.5                                             
      1        Alfred     M      14      69       112.5                                             
      1        Alfred     M      14      69       112.5                                             
    ...                                                                                             
    100019017  Alfred     M      14      69       112.5                                             
    100019018  Alfred     M      14      69       112.5                                             
    100019019  Alfred     M      14      69       112.5  * 19,020 Distinct                          
                                                                                                    
                                                                                                    
    WORK.HAVE2 total obs=199,982,919                                                                
                                                                                                    
     KEY        NAME     SEX    AGE    HEIGHT    WEIGHT                                             
                                                                                                    
      2        Alfred     M      14      69       112.5                                             
      2        Alfred     M      14      69       112.5                                             
      2        Alfred     M      14      69       112.5                                             
      2        Alfred     M      14      69       112.5                                             
     ...                                                                                            
    100000000  Alfred     M      14      69       112.5                                             
    100000001  Alfred     M      14      69       112.5                                             
    100000002  Alfred     M      14      69       112.5                                             
    ...,                                                                                            
      2        Alfred     M      14      69       112.5                                             
      2        Alfred     M      14      69       112.5                                             
      2        Alfred     M      14      69       112.5                                             
    ...                                                                                             
    100001899  Alfred     M      14      69       112.5                                             
    100001900  Alfred     M      14      69       112.5                                             
    100001901  Alfred     M      14      69       112.5  * 1,902 Distinct                           
                                                                                                    
    *            _               _                                                                  
      ___  _   _| |_ _ __  _   _| |_                                                                
     / _ \| | | | __| '_ \| | | | __|                                                               
    | (_) | |_| | |_| |_) | |_| | |_                                                                
     \___/ \__,_|\__| .__/ \__,_|\__|                                                               
                    |_|                                                                             
    ;                                                                                               
                                                                                                    
    WORK.WANT total obs=1,902                                                                       
                                                                                                    
         KEY        NAME     SEX    AGE    HEIGHT    WEIGHT                                         
                                                                                                    
      100000000    Alfred     M      14      69       112.5                                         
      100000010    Alfred     M      14      69       112.5                                         
      100000020    Alfred     M      14      69       112.5                                         
      100000030    Alfred     M      14      69       112.5                                         
      100000040    Alfred     M      14      69       112.5                                         
       ....                                                                                         
       ....                                                                                         
                                                                                                    
      100001899    Alfred     M      14      69       112.5                                         
      100001900    Alfred     M      14      69       112.5                                         
      100001901    Alfred     M      14      69       112.5                                         
                                                                                                    
    *          _       _   _                                                                        
     ___  ___ | |_   _| |_(_) ___  _ __                                                             
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                            
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                           
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                           
                                                                                                    
    ;                                                                                               
                                                                                                    
                                                                                                    
    proc sql;                                                                                       
      create index key on have1                                                                     
    ;quit;                                                                                          
                                                                                                    
    /*                                                                                              
    NOTE: Simple index key has been defined.                                                        
    NOTE: PROCEDURE SQL used (Total process time):                                                  
          real time           1:01.07                                                               
          cpu time            2:40.58                                                               
    */                                                                                              
                                                                                                    
    proc sql;                                                                                       
      create                                                                                        
        table want as                                                                               
      select                                                                                        
        l.*                                                                                         
      from                                                                                          
        have1 as l, have2 as r                                                                      
      where                                                                                         
       l.key = r.key                                                                                
    ;quit;                                                                                          
                                                                                                    
    NOTE: Table WORK.WANT created, with 1902 rows and 6 columns.                                    
    NOTE: PROCEDURE SQL used (Total process time):                                                  
          real time           2:50.08                                                               
          cpu time            6:19.09                                                               
                                                                                                    
