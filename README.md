# utl_find_most_recent_log_file_in_a_directory_of_log_files
Find most recent log file in a directory of log files. Will work with Unix but you will have to add code to hadle the mutiple formats for dates.

    ```  Find most recent log file in a directory of log files.                                                                                                       ```
    ```                                                                                                                                                               ```
    ```  Will work with Unix but you will have to add code to hadle the mutiple formats for dates.                                                                    ```
    ```                                                                                                                                                               ```
    ```  INPUT                                                                                                                                                        ```
    ```  ======                                                                                                                                                       ```
    ```      d:/log                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```         pgm2.log           11/14/2017                                                                                                                         ```
    ```         pgm1.log           11/2/2017                                                                                                                          ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  WORKING CODE                                                                                                                                                 ```
    ```  ============                                                                                                                                                 ```
    ```     %ls_l(d:/log);                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```     proc sql;                                                                                                                                                 ```
    ```        reset outobs=1;                                                                                                                                        ```
    ```        create table most_recent as                                                                                                                            ```
    ```        select * from ls_l                                                                                                                                     ```
    ```        order by moddate descending                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  OUTPUT                                                                                                                                                       ```
    ```  ======                                                                                                                                                       ```
    ```     Lstname             =  pgmlog2.log                                                                                                                        ```
    ```     Directory           =  d:\log\pgmlog2.log                                                                                                                 ```
    ```     Record_type         =  V                                                                                                                                  ```
    ```     Max_record_length   =  384                                                                                                                                ```
    ```     Filesize_bytes      =  1353                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```     Modification_date   =  14Nov2017:14:51:45    ** most recent;                                                                                              ```
    ```                                                                                                                                                               ```
    ```     Creation_date       =  09Aug2017:19:33:39                                                                                                                 ```
    ```     Moddate             =  1826290264                                                                                                                         ```
    ```     Createdate          =  1817926383                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```  see                                                                                                                                                          ```
    ```  https://goo.gl/9Un1SZ                                                                                                                                        ```
    ```  https://communities.sas.com/t5/General-SAS-Programming/Find-recent-log-file-of-a-SAS-Job/m-p/413581                                                          ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  *                _               _       _                                                                                                                   ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _                                                                                                            ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |                                                                                                           ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |                                                                                                           ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```    You need to have your own directory                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  *          _       _   _                                                                                                                                     ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                          ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                                         ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                        ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  %macro ls_l(dir)/des="directory list when SAS turns off the opersting system suppot";                                                                        ```
    ```                                                                                                                                                               ```
    ```     %put %sysfunc(ifc(%sysevalf(%superq(dir)=,boolean),**** Please Provide a directory    ****,));                                                            ```
    ```                                                                                                                                                               ```
    ```      %let res= %eval                                                                                                                                          ```
    ```      (                                                                                                                                                        ```
    ```          %sysfunc(ifc(%sysevalf(%superq(dir )=,boolean),1,0))                                                                                                 ```
    ```      );                                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```       %if &res = 0 %then %do;                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```          data ls_l(keep=lstname fyl1 fyl2 fyl3 fyl4 fyl5 fyl6 moddate createdate                                                                              ```
    ```             rename=(                                                                                                                                          ```
    ```               fyl1 =Directory                                                                                                                                 ```
    ```               fyl2 =Record_type                                                                                                                               ```
    ```               fyl3 =Max_record_length                                                                                                                         ```
    ```               fyl4 =Filesize_bytes                                                                                                                            ```
    ```               fyl5 =Modification_date                                                                                                                         ```
    ```               fyl6 =Creation_date                                                                                                                             ```
    ```           ));                                                                                                                                                 ```
    ```             length infoname infoval $60;                                                                                                                      ```
    ```             retain lstname;                                                                                                                                   ```
    ```             length lstname $64;                                                                                                                               ```
    ```             rc=filename("_mydir","&dir");                                                                                                                     ```
    ```             did=dopen("_mydir");                                                                                                                              ```
    ```             if did > 0 then do;                                                                                                                               ```
    ```                memcount=dnum(did);                                                                                                                            ```
    ```                close=fclose(did);                                                                                                                             ```
    ```                do i=1 to memcount;                                                                                                                            ```
    ```                   lstname=dread(did,i);                                                                                                                       ```
    ```                   rc=filename("_myfyl",cats("&dir",'/',lstname));                                                                                             ```
    ```                   fid=fopen("_myfyl");                                                                                                                        ```
    ```                   if fid > 0 then do;                                                                                                                         ```
    ```                      infonum=foptnum(fid);                                                                                                                    ```
    ```                       array nfo[6] $255 fyl1-fyl6;                                                                                                            ```
    ```                       do j=1 to infonum;                                                                                                                      ```
    ```                          infoname=foptname(fid,j);                                                                                                            ```
    ```                          infoval=finfo(fid,infoname);                                                                                                         ```
    ```                          nfo[j]=finfo(fid,infoname);                                                                                                          ```
    ```                       end;                                                                                                                                    ```
    ```                       if i=1 then do;                                                                                                                         ```
    ```                          put "Directory &dir" /;                                                                                                              ```
    ```                       end;                                                                                                                                    ```
    ```                       moddate=input(nfo[5],datetime17.);                                                                                                      ```
    ```                       createdate=input(nfo[6],datetime17.);                                                                                                   ```
    ```                       *put @1 fylnum comma16. @20 nfo[5] $20. nfo[6] $20. lstname $64.;                                                                       ```
    ```                       *put nfo[2] $8. nfo[3] $8. @19 nfo[4] $10. @32 nfo[5] $15.  @50  nfo[6] $15.-r @70  lstname ;                                           ```
    ```                       close=fclose(fid);                                                                                                                      ```
    ```                       output;                                                                                                                                 ```
    ```                   end;                                                                                                                                        ```
    ```                   else do;                                                                                                                                    ```
    ```                      put "subdirectory or &dir/" lstname " no files in directory or file cannot be opened";                                                   ```
    ```                                                                                                                                                               ```
    ```                   end;                                                                                                                                        ```
    ```                end;                                                                                                                                           ```
    ```            end;                                                                                                                                               ```
    ```            else do;                                                                                                                                           ```
    ```            put "&dir does not exist or cannot be opened";                                                                                                     ```
    ```          end;                                                                                                                                                 ```
    ```       %end;                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```     run;quit;                                                                                                                                                 ```
    ```  %mend ls_l;                                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```  %ls_l(d:/log);                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  proc sql;                                                                                                                                                    ```
    ```    reset outobs=1;                                                                                                                                            ```
    ```    create                                                                                                                                                     ```
    ```        table most_recent as                                                                                                                                   ```
    ```    select                                                                                                                                                     ```
    ```        *                                                                                                                                                      ```
    ```    from                                                                                                                                                       ```
    ```        ls_l                                                                                                                                                   ```
    ```    order                                                                                                                                                      ```
    ```        by moddate descending                                                                                                                                  ```
    ```  ;quit;                                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  data _null_;                                                                                                                                                 ```
    ```     set most_recent;                                                                                                                                          ```
    ```     put (_all_) (= / $);                                                                                                                                      ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  Lstname             =pgmlog2.log                                                                                                                             ```
    ```  Directory           =d:\log\pgmlog2.log                                                                                                                      ```
    ```  Record_type         =V                                                                                                                                       ```
    ```  Max_record_length   =384                                                                                                                                     ```
    ```  Filesize_bytes      =1353                                                                                                                                    ```
    ```  Modification_date   =14Nov2017:14:51:45                                                                                                                      ```
    ```  Creation_date       =09Aug2017:19:33:39                                                                                                                      ```
    ```  Moddate             =1826290264                                                                                                                              ```
    ```  Createdate          =1817926383                                                                                                                              ```
    ```                                                                                                                                                               ```

