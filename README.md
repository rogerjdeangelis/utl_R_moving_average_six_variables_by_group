# utl_R_moving_average_six_variables_by_group
Darta analytics. Machine learning. SAS and R. Just two lines of R code to perform a moving average of six variables by group handles alignment and gaps 

    ```  Arbitrary moving average of six variables by Stock (right averageing and handles gaps)  ```
    ```    ```
    ```     WORKING CODE (moving average of 3)  ```
    ```    ```
    ```      WPS/PROC-R SAS/IML/R (basically two lines of code)  ```
    ```    ```
    ```         myfun = function(x) rollmean(x, k = 3, fill = NA, align = "right");  ```
    ```         have %>%  ```
    ```            group_by(STOCK) %>%  ```
    ```            mutate_each(funs(myfun), OPEN ,HIGH ,LOW ,CLOSE ,VOLUME ,ADJCLOSE)  ```
    ```            -> wantwps;  ```
    ```    ```
    ```  SAS Forum  ```
    ```  https://goo.gl/XXnGWE  ```
    ```  https://communities.sas.com/t5/Base-SAS-Programming/window-operation-by-group/m-p/395583  ```
    ```    ```
    ```  https://goo.gl/SfER1q  ```
    ```  https://stackoverflow.com/questions/35991288/moving-averages-on-multiple-columns-grouped-data  ```
    ```    ```
    ```  MidnightDataGeek  ```
    ```  https://stackoverflow.com/users/3242390/midnightdatageek  ```
    ```    ```
    ```    ```
    ```  HAVE  ```
    ```  ====  ```
    ```    ```
    ```    Up to 40 obs SD1.HAVE total obs=44  ```
    ```                                                                                                   RULES (rolling 3)  ```
    ```    Obs    MONTH    STOCK         OPEN      HIGH       LOW      CLOSE      VOLUME     ADJCLOSE     for ADJCLOSE  ```
    ```    ```
    ```      1       1     IBM           72.01     74.20     65.70     70.40     11771613      67.72  ```
    ```      2       2     IBM          107.64    109.30     95.76     98.12      9487368      94.20  ```
    ```      3       3     IBM          113.50    115.40    101.56    105.21      7934257     100.62      262.54/3 =87.51  ```
    ```    ```
    ```                                                                         29193238/3  ```
    ```                                                                        =9731079.33  ```
    ```    ```
    ```      4       4     IBM          115.85    119.90    110.96    111.80      7549104     106.92  ```
    ```      5       5     IBM           84.50    116.75     83.75    112.00      9872776     106.86  ```
    ```     10      10     IBM          122.87    124.37    114.12    118.50      5188438      21.26  ```
    ```    ...  ```
    ```     11       1     Intel         32.36     34.60     30.35     30.52     59486895      29.47  ```
    ```     12       2     Intel         33.33     34.51     31.66     33.54     48305094      32.39  ```
    ```    ...  ```
    ```    ```
    ```    29       1      Microsoft     25.23     26.09     24.64     25.80     62699752      25.46  ```
    ```    30       2      Microsoft     26.35     26.75     24.84     25.71     77670488      22.56  ```
    ```    31       3      Microsoft     24.98     26.51     23.60     25.64     77050900      22.37      70.39/3  = 23.46  ```
    ```    32       4      Microsoft     47.93     49.10     23.30     23.70     73871379      20.68  ```
    ```    ```
    ```    ```
    ```  WANT  ```
    ```  ====  ```
    ```    ```
    ```  Obs    MONTH    STOCK          OPEN      HIGH       LOW     CLOSE         VOLUME    ADJCLOSE  ```
    ```    ```
    ```    1     1.00    IBM             .         .         .         .              .          .  ```
    ```    2     2.00    IBM             .         .         .         .              .          .  ```
    ```    3     3.00    IBM           97.72     99.63     87.67     91.24     9731079.33      87.51    Follows the rules  ```
    ```    ```
    ```   21    11.00    Intel         67.21     77.54     65.54     70.75    91572380.67       5.47  ```
    ```   22    12.00    Intel         62.25     70.00     60.29     67.67    79083094.33       3.44  ```
    ```   23    13.00    Intel         46.04     53.50     44.17     51.50    81879893.00       2.07  ```
    ```   24    14.00    Intel         30.75     36.75     28.75     35.50    67897107.67       0.87  ```
    ```    ```
    ```   25     1.00    Microsoft       .         .         .         .              .          .  ```
    ```   26     2.00    Microsoft       .         .         .         .              .          .  ```
    ```   27     3.00    Microsoft     25.52     26.45     24.36     25.72    72473713.33      23.46  ```
    ```   28     4.00    Microsoft     33.09     34.12     23.91     25.02    76197589.00      21.87  ```
    ```    ```
    ```    ```
    ```  *                _              _       _  ```
    ```   _ __ ___   __ _| | _____    __| | __ _| |_ __ _  ```
    ```  | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |  ```
    ```  | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |  ```
    ```  |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  options validvarname=upcase;  ```
    ```  libname sd1 "d:/sd1";  ```
    ```  data sd1.have;  ```
    ```    retain month 0;  ```
    ```    set sashelp.stocks(where=(uniform(5733)<.05));  ```
    ```    by stock;  ```
    ```    month=month+1;  ```
    ```    output;  ```
    ```    if last.stock then month=0;  ```
    ```    drop date;  ```
    ```  run;quit;  ```
    ```    ```
    ```  *          _       _   _  ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __  ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \  ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |  ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  %utl_submit_wps64('  ```
    ```  libname sd1 sas7bdat "d:/sd1";  ```
    ```  options set=R_HOME "C:/Program Files/R/R-3.3.2";  ```
    ```  libname wrk sas7bdat "%sysfunc(pathname(work))";  ```
    ```  proc r;  ```
    ```  submit;  ```
    ```  source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);  ```
    ```  library(haven);  ```
    ```  have<-read_sas("d:/sd1/have.sas7bdat");  ```
    ```  library(dplyr);  ```
    ```  library(zoo);  ```
    ```  myfun = function(x) rollmean(x, k = 3, fill = NA, align = "right");  ```
    ```  have %>%  ```
    ```     group_by(STOCK) %>%  ```
    ```     mutate_each(funs(myfun), OPEN ,HIGH ,LOW ,CLOSE ,VOLUME ,ADJCLOSE)  ```
    ```     -> wantwps;  ```
    ```  endsubmit;  ```
    ```  import r=wantwps  data=wrk.wantwps;  ```
    ```  run;quit;  ```
    ```  ');  ```
    ```    ```
    ```  proc print data=wantwps width=min;  ```
    ```  format _numeric_ 18.2;  ```
    ```  run;quit;  ```
    ```    ```
    ```    ```
    ```  Obs    MONTH    STOCK          OPEN      HIGH       LOW     CLOSE         VOLUME    ADJCLOSE  ```
    ```    ```
    ```    1     1.00    IBM             .         .         .         .              .          .  ```
    ```    2     2.00    IBM             .         .         .         .              .          .  ```
    ```    3     3.00    IBM           97.72     99.63     87.67     91.24     9731079.33      87.51  ```
    ```    4     4.00    IBM          112.33    114.87    102.76    105.04     8323576.33     100.58  ```
    ```    5     5.00    IBM          104.62    117.35     98.76    109.67     8452045.67     104.80  ```
    ```    6     6.00    IBM          104.28    116.76     98.90    110.37     7740811.33     105.31  ```
    ```    7     7.00    IBM          103.54    120.79     98.40    116.19     7336611.33     110.49  ```
    ```    8     8.00    IBM          109.91    116.79    102.73    111.60     5881506.00      81.51  ```
    ```    9     9.00    IBM          106.08    112.83     99.40    108.12     5963624.33      54.00  ```
    ```   10    10.00    IBM          109.16    110.29    100.96    104.54     5580935.67      20.27  ```
    ```   11     1.00    Intel           .         .         .         .              .          .  ```
    ```   12     2.00    Intel           .         .         .         .              .          .  ```
    ```   13     3.00    Intel         32.53     35.30     31.19     33.03    54468474.00      31.82  ```
    ```   14     4.00    Intel         30.93     33.13     27.39     29.67    59959624.33      28.52  ```
    ```   15     5.00    Intel         45.69     49.58     41.38     44.06    57974996.00      29.93  ```
    ```   16     6.00    Intel         70.33     79.38     66.09     71.90    58190615.00      28.15  ```
    ```   17     7.00    Intel         87.29     97.27     83.78     90.96    55437889.00      27.78  ```
    ```   18     8.00    Intel        103.42    116.48    100.36    109.04    70819409.67      20.76  ```
    ```   19     9.00    Intel         89.27    100.54     85.96     91.19    90370393.00      13.91  ```
    ```   20    10.00    Intel         86.25    100.21     85.00     91.90    96005417.67       9.33  ```
    ```   21    11.00    Intel         67.21     77.54     65.54     70.75    91572380.67       5.47  ```
    ```   22    12.00    Intel         62.25     70.00     60.29     67.67    79083094.33       3.44  ```
    ```   23    13.00    Intel         46.04     53.50     44.17     51.50    81879893.00       2.07  ```
    ```   24    14.00    Intel         30.75     36.75     28.75     35.50    67897107.67       0.87  ```
    ```   25     1.00    Microsoft       .         .         .         .              .          .  ```
    ```   26     2.00    Microsoft       .         .         .         .              .          .  ```
    ```   27     3.00    Microsoft     25.52     26.45     24.36     25.72    72473713.33      23.46  ```
    ```   28     4.00    Microsoft     33.09     34.12     23.91     25.02    76197589.00      21.87  ```
    ```   29     5.00    Microsoft     44.33     47.98     35.50     37.85    72199534.00      23.66  ```
    ```   30     6.00    Microsoft     58.56     63.20     50.05     52.36    74515724.67      26.23  ```
    ```   31     7.00    Microsoft     62.85     68.52     60.24     64.13    78339370.00      27.89  ```
    ```   32     8.00    Microsoft     66.16     69.76     59.92     62.83    81440385.67      27.32  ```
    ```   33     9.00    Microsoft     67.89     70.37     57.63     60.62    74643576.67      26.36  ```
    ```   34    10.00    Microsoft    106.10    107.33     87.66     91.00    75290461.00      28.69  ```
    ```    ```
    ```    ```
    ```    ```
