# utl-simple-example-of-sql-array
Simple example of sql arrays
    %let pgm=utl-simple-example-of-sql-arrays;

    Simple example of sql arrays

    This might be faster than you think, compilers/interpreters like repeatative code.

    github
    https://tinyurl.com/44s9f49j
    https://github.com/rogerjdeangelis/utl-simple-example-of-sql-array

    StackOverflow SAS
    https://tinyurl.com/35vzaszw
    https://stackoverflow.com/questions/75896060/incrementing-inside-sas-proc-sql H

    many macros
    https://tinyurl.com/2w6aejec
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data have ;
      set sashelp.stocks (rename=(
         OPEN     = cmr1
         HIGH     = cmr2
         LOW      = cmr3
         CLOSE    = cmr4
         VOLUME   = cmr5
         ADJCLOSE = cmr6
         ));
      date = year(date);
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* last 41 obs from WORK.HAVE total obs=699 05APR2023:08:04:25                                                            */
    /*                                                                                                                        */
    /* Obs      STOCK      CMR1      CMR2      CMR3      CMR4        CMR5      CMR6                                           */
    /*                                                                                                                        */
    /* 659    Microsoft    87.12     88.25     75.00     87.00     76913920    0.53                                           */
    /* 660    Microsoft    81.75     89.25     76.00     87.00     92717257    0.53                                           */
    /* 661    Microsoft    68.25     86.00     68.00     81.75    121160872    0.49                                           */
    /* 662    Microsoft    59.00     69.50     57.75     68.50     67441440    0.41                                           */
    /* 663    Microsoft    54.75     60.50     54.00     58.75     60648208    0.35                                           */
    /* 664    Microsoft    53.13     57.00     51.50     54.75     62716960    0.33                                           */
    /* 665    Microsoft    60.25     60.75     51.25     53.00     51809454    0.32                                           */
    /* 666    Microsoft    55.38     61.00     51.00     60.50     60905745    0.37                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %array(_vrs,values=%utl_varlist(have,keep=cmr:));
    %array(_bds,values=1-&_vrsn);

    %put &=_vrs1;  /*---- cmr1 ----*/
    %put &=_vrsn;  /*---- 6    ----*/

    %put &=_bds1;  /*---- 1    ----*/
    %put &=_bdsn;  /*---- 6    ----*/

    proc sql;
      create
        table want as
      select
        distinct
          stock
         ,date
         ,%do_over(_vrs _bds, phrase = %str(sum(?_vrs) as bads_?_bds), between=comma )
      from
        have
      group by
        stock, date
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Up to 40 obs from last table WORK.WANT total obs=60 05APR2023:08:44:44                                                 */
    /* Obs    STOCK    DATE     BADS_1     BADS_2     BADS_3     BADS_4      BADS_5       BADS_6                              */
    /*                                                                                                                        */
    /*   1    IBM      1986     656.89     681.00     621.38     643.99      33621593     107.29                              */
    /*   2    IBM      1987    1734.51    1847.15    1616.14    1729.89     103475428     294.40                              */
    /*   3    IBM      1988    1402.97    1466.72    1328.97    1406.34      72801028     248.17                              */
    /*   4    IBM      1989    1357.60    1395.99    1288.10    1330.22      80016711     243.61                              */
    /*   5    IBM      1990    1289.60    1358.34    1243.98    1306.97      79705512     250.95                              */
    /*   6    IBM      1991    1279.47    1340.10    1199.22    1257.10      87469737     251.93                              */
    /*   7    IBM      1992    1026.48    1069.59     950.60     987.36     100892927     208.20                              */
    /*   8    IBM      1993     595.02     631.42     551.16     596.15     115440423     132.17                              */
    /*   9    IBM      1994     742.25     795.74     710.75     762.01     119070975     172.66                              */
    /*  10    IBM      1995    1087.98    1162.72    1035.60    1105.09     155255217     253.67                              */
    /*  11    IBM      1996    1380.99    1518.86    1308.39    1442.12     169360856     334.61                              */
    /*  ...                                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*                               _           _                 _
      __ _  ___ _ __   ___ _ __ __ _| |_ ___  __| |   ___ ___   __| | ___
     / _` |/ _ \ `_ \ / _ \ `__/ _` | __/ _ \/ _` |  / __/ _ \ / _` |/ _ \
    | (_| |  __/ | | |  __/ | | (_| | ||  __/ (_| | | (_| (_) | (_| |  __/
     \__, |\___|_| |_|\___|_|  \__,_|\__\___|\__,_|  \___\___/ \__,_|\___|
     |___/
    */

    data _null_;

       %do_over(_vrs _bds, phrase = %str(put ",sum(?_vrs) as bads_?_bds";));

    run;quit;

    /*************************************************************************************************************************
    /*
    /* ,sum(CMR1) as bads_1
    /* ,sum(CMR2) as bads_2
    /* ,sum(CMR3) as bads_3
    /* ,sum(CMR4) as bads_4
    /* ,sum(CMR5) as bads_5
    /* ,sum(CMR6) as bads_6
    /*
    /*************************************************************************************************************************

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
