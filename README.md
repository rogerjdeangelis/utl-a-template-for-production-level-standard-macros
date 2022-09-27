# utl-a-template-for-production-level-standard-macros
Template for production level standard macros
    %let pgm=utl-a-template-for-production-level-standard-macros;                                                                             
                                                                                                                                              
    Template for production level standard macros                                                                                             
                                                                                                                                              
    Standard macro at the end of this message.                                                                                                
    Standard macros are high risk code and require more documentation that normal programs                                                    
                                                                                                                                              
    Problem                                                                                                                                   
                                                                                                                                              
    DEVELOP A STANDARD MACRO FOR THE INTERSECTION OF WORDS IN TWO LISTS;                                                                      
                                                                                                                                              
                                                                      |   Rules                                                               
                                                                      |   Common Words                                                        
      SRC                              TRG                            |   RESULT                                                              
                                                                      |                                                                       
      one two three four               two three                      |   two three                                                           
      five six seven eight             one two three four             |                                                                       
      one two three four five          five                           |   five                                                                
      roger suzie roger suzie roger    roger roger suzie suzie roger  |   roger suzie                                                         
      sas wps                          sas wps                        |   sas wps                                                             
      taco taco taco                   taco                           |   taco                                                                
      taco                             taco taco                      |   taco                                                                
                                                                                                                                              
     Four SOLUTIONS                                                                                                                           
                                                                                                                                              
        1. SAS macro                                                                                                                          
        2. SAS error detect                                                                                                                   
                                                                                                                                              
    /*                                                                                                                                        
     _                   _                                                                                                                    
    (_)_ __  _ __  _   _| |_                                                                                                                  
    | | `_ \| `_ \| | | | __|                                                                                                                 
    | | | | | |_) | |_| | |_                                                                                                                  
    |_|_| |_| .__/ \__,_|\__|                                                                                                                 
            |_|                                                                                                                               
    */                                                                                                                                        
                                                                                                                                              
    * find common words in source and target list;                                                                                            
    libname sd1 "d:/sd1";                                                                                                                     
    data sd1.have;                                                                                                                            
      informat src trg $200.;                                                                                                                 
      infile cards delimiter=',';                                                                                                             
      input src trg;                                                                                                                          
    cards4;                                                                                                                                   
    one two three four,two three                                                                                                              
    five six seven eight,one two three four                                                                                                   
    one two three four five,five                                                                                                              
    roger suzie roger suzie roger,roger roger suzie suzie roger                                                                               
    sas wps,sas wps                                                                                                                           
    taco taco taco,taco                                                                                                                       
    taco,taco taco                                                                                                                            
    ;;;;                                                                                                                                      
    run;quit;                                                                                                                                 
                                                                                                                                              
    Up to 40 obs from SD1.HAVE total obs=7 25SEP2022:08:10:46                                                                                 
                                                                                                                                              
    Obs    SRC                              TRG                                                                                               
                                                                                                                                              
     1     one two three four               two three                                                                                         
     2     five six seven eight             one two three four                                                                                
     3     one two three four five          five                                                                                              
     4     roger suzie roger suzie roger    roger roger suzie suzie roger                                                                     
     5     sas wps                          sas wps                                                                                           
     6     taco taco taco                   taco                                                                                              
     7     taco                             taco taco                                                                                         
                                                                                                                                              
    /*           _               _                                                                                                            
      ___  _   _| |_ _ __  _   _| |_                                                                                                          
     / _ \| | | | __| `_ \| | | | __|                                                                                                         
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                          
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                         
                    |_|                                                                                                                       
    */                                                                                                                                        
                                                                                                                                              
    Up to 40 obs from WANT_DATASTEP total obs=7 25SEP2022:08:22:40                                                                            
                                                                                                                                              
    Obs    RESULT         SRC                              TRG                                                                                
                                                                                                                                              
     1     two three      one two three four               two three                                                                          
     2                    five six seven eight             one two three four                                                                 
     3     five           one two three four five          five                                                                               
     4     roger suzie    roger suzie roger suzie roger    roger roger suzie suzie roger                                                      
     5     sas wps        sas wps                          sas wps                                                                            
     6     taco           taco taco taco                   taco                                                                               
     7     taco           taco                             taco taco                                                                          
                                                                                                                                              
                                                                                                                                              
    /*                                                                                                                                        
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                        
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|                                                                                                       
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                       
    | .__/|_|  \___/ \___\___||___/___/                                                                                                       
    |_|                                                                                                                                       
     _                                                                                                                                        
    / |    ___  __ _ ___   _ __ ___   __ _  ___ _ __ ___                                                                                      
    | |   / __|/ _` / __| | `_ ` _ \ / _` |/ __| `__/ _ \                                                                                     
    | |_  \__ \ (_| \__ \ | | | | | | (_| | (__| | | (_) |                                                                                    
    |_(_) |___/\__,_|___/ |_| |_| |_|\__,_|\___|_|  \___/                                                                                     
                                                                                                                                              
    */                                                                                                                                        
                                                                                                                                              
    proc datasets lib=work mt=data kill nodetails nolist;                                                                                     
    run;quit;                                                                                                                                 
                                                                                                                                              
    * only need if repeated interactive runs ;                                                                                                
    %symdel arg_src arg_trg arg_result / nowarn;                                                                                              
                                                                                                                                              
    libname sd1 "d:/sd1";                                                                                                                     
                                                                                                                                              
    options sasautos=("c:/oto" sasautos);                                                                                                     
    data want_macro;                                                                                                                          
      %inc "c:/oto/utl_intersectWords.sas";                                                                                                   
      set sd1.have;                                                                                                                           
                                                                                                                                              
      %utl_intersectWords(                                                                                                                    
          src      /* source list of words  */                                                                                                
         ,trg      /* target list of words  */                                                                                                
         ,result   /* returned common words */                                                                                                
      );                                                                                                                                      
                                                                                                                                              
    proc print data=want_macro;                                                                                                               
    run;quit;                                                                                                                                 
                                                                                                                                              
    /*___                                                     _      _            _                                                           
    |___ \     ___  __ _ ___    ___ _ __ _ __ ___  _ __    __| | ___| |_ ___  ___| |_                                                         
      __) |   / __|/ _` / __|  / _ \ `__| `__/ _ \| `__|  / _` |/ _ \ __/ _ \/ __| __|                                                        
     / __/ _  \__ \ (_| \__ \ |  __/ |  | | | (_) | |    | (_| |  __/ ||  __/ (__| |_                                                         
    |_____(_) |___/\__,_|___/  \___|_|  |_|  \___/|_|     \__,_|\___|\__\___|\___|\__|                                                        
    */                                                                                                                                        
                                                                                                                                              
    * run with missing arguments;                                                                                                             
    data tst;                                                                                                                                 
      set sd1.have;                                                                                                                           
      %utl_intersectWords;                                                                                                                    
    run;quit;                                                                                                                                 
                                                                                                                                              
    **** Please Provide blank separated source word list ****                                                                                 
    **** Please Provide blank separated target word list ****                                                                                 
    **** Please Provide return variable  ****                                                                                                 
                                                                                                                                              
    +---------------------------------------------+                                                                                           
    |                                             |                                                                                           
    | Sample Call                                 |                                                                                           
    |                                             |                                                                                           
    | %utl_intersectWords(src,trg,result)         |                                                                                           
    |                                             |                                                                                           
    | %utl_intersectWords(                        |                                                                                           
    |    src     ==>  source list of words        |                                                                                           
    |   ,trg     ==>  target list of words        |                                                                                           
    |   ,result  ==>  common words                |                                                                                           
    | )                                           |                                                                                           
    |                                             |                                                                                           
    +---------------------------------------------+                                                                                           
                                                                                                                                              
    ERROR: Stoped incorrect invocation of macro utl_intersectWords                                                                            
                                                                                                                                              
                                                                                                                                              
    /*   _                  _               _                                                                                                 
     ___| |_ __ _ _ __   __| | __ _ _ __ __| |  _ __ ___   __ _  ___ _ __ ___                                                                 
    / __| __/ _` | `_ \ / _` |/ _` | `__/ _` | | `_ ` _ \ / _` |/ __| `__/ _ \                                                                
    \__ \ || (_| | | | | (_| | (_| | | | (_| | | | | | | | (_| | (__| | | (_) |                                                               
    |___/\__\__,_|_| |_|\__,_|\__,_|_|  \__,_| |_| |_| |_|\__,_|\___|_|  \___/                                                                
                                                                                                                                              
    */                                                                                                                                        
                                                                                                                                              
    * put this macro in your autocall library c:/oto/utl_intersectWords.sas or highlight macro only and submit;                               
                                                                                                                                              
    filename ft15f001 "c:/oto/utl_intersectWords.sas";                                                                                        
    parmcards4;                                                                                                                               
    /**************************************************************************************************************************/              
    /*                                                                                                                        */              
    /*  Code token=utl (because this is a high risk standard macro much more detail is in the header)                         */              
    /*                 ( using fixed paths for now c: for global folders and d: for working development folders)              */              
    /*                                                                                                                        */              
    /*  OPSYS                  : Win 10 64bit workstation                                                                     */              
    /*                                                                                                                        */              
    /*  PURPOSE                : Create a list of distinct common words contained in two lists                                */              
    /*                                                                                                                        */              
    /*  LANGUAGE               : SAS 9.4M7 64bit or WPS 4.2 64bit                                                             */              
    /*                                                                                                                        */              
    /*  OPSYS;                 : Win 10 64bit workstation                                                                     */              
    /*                                                                                                                        */              
    /*  AUTOCALL LIBRARY       : c:/oto/utl_intersectWords.sas                                                                */              
    /*                         : Macro must be called from within a datastep that already has the two lists                   */              
    /*                                                                                                                        */              
    /*  PROGRAM                : d:/utl/sas intersectWords.sas                                                                */              
    /*                                                                                                                        */              
    /*  PROGRAM LOG            : d:/utl/log/utl_intersectWords.log                                                            */              
    /*                                                                                                                        */              
    /*  PROGRAM LIST           : d:/utl/lst/utl_intersectWords.lst                                                            */              
    /*                                                                                                                        */              
    /*  REQUESTOR              : Compucraft Principle Statistician rdeangelis@compucraft.com                                  */              
    /*                                                                                                                        */              
    /*  PRODUCTION PROGRAMMER  : rdeangelis@compucraft.com                                                                    */              
    /*                                                                                                                        */              
    /*  VERSIONING             : c:\ver\utl_intersectWords[datetime].sas                                                      */              
    /*                                                                                                                        */              
    /*  VALIDATED              : Yes                                                                                          */              
    /*                                                                                                                        */              
    /*  RISK LEVEL             : High                                                                                         */              
    /*                                                                                                                        */              
    /*  VALIDATION PROGRAM     : d:\utl\sas\utl_intersectWordsDP.sas                                                          */              
    /*                                                                                                                        */              
    /*  ISSUE LOG              : d:\utl\xls\utl_intersectWordsIsu.xlsx                                                        */              
    /*                                                                                                                        */              
    /*  DEPENDENCIES           : None                                                                                         */              
    /*                                                                                                                        */              
    /*  VALIDATION PROGRAMMER  : jsmith@compucraft.com                                                                        */              
    /*                         : All standard macros go through this testing                                                  */              
    /*                         : User Requirements                                                                            */              
    /*                         : Functional Requirements                                                                      */              
    /*                         : Unit Test Plans                                                                              */              
    /*                         : Configuration Management Requirements                                                        */              
    /*                                                                                                                        */              
    /*  DOCUMENTATION          : d\utl\pdf\utl_intersectWords.pdf                                                             */              
    /*                                                                                                                        */              
    /*  R/PYTHON DEPENDENCIES : None                                                                                          */              
    /*                                                                                                                        */              
    /*  EXTERNAL MACROS       : None                                                                                          */              
    /*                                                                                                                        */              
    /*  INTERNAL MACROS       : None                                                                                          */              
    /*                                                                                                                        */              
    /*                                                                                                                        */              
    /**************************************************************************************************************************/              
    /*                                                                                                                        */              
    /* VERSION HISTORY                                                                                                        */              
    /*                                                                                                                        */              
    /*   Programmer                            Date              Description of Changes                                       */              
    /*                                                                                                                        */              
    /*   roger.deangelis@compucraft.com       2022/05/28         Creation                                                     */              
    /*                                                                                                                        */              
    /**************************************************************************************************************************/              
    /*                                                                                                                        */              
    /* PROCESS FLOW                                                                                                           */              
    /*                                                                                                                        */              
    /*     1. Macro must be called from within a datastep that already contains the two lists.                                */              
    /*     2. If macro any macro argument is missing then echo the missing arguments(s) and echo a sample call.               */              
    /*     3. Solution is case sensitive and puctuation sensitive. Words must match exactly.echo a sample call.               */              
    /*     4. If invocation is complete then snf both lista are blank separated then do                                       */              
    /*         a. Create temporary variables for use in the macro. Temproray variablles are not outputted to final table.     */              
    /*            This greatly eliminates the possibility of conflicts with the call programe                                 */              
    /*         b. Since temporary variables are not allowed in a do statement we will save _n_, then used _n_ as iterator.    */              
    /*         c. Check each source word to see if it is in the target list. If it is and has not previously been found       */              
    /*            then add it                                                                                                 */              
    /*         d, Restore _n_                                                                                                 */              
    /*                                                                                                                        */              
    /**************************************************************************************************************************/              
    /*                                                                             _               _                          */              
    /*  _                   _      _ __  _ __ ___   ___ ___  ___ ___    ___  _   _| |_ _ __  _   _| |_                        */              
    /* (_)_ __  _ __  _   _| |_   | `_ \| `__/ _ \ / __/ _ \/ __/ __|  / _ \| | | | __| `_ \| | | | __|                       */              
    /* | | `_ \| `_ \| | | | __|  | |_) | | | (_) | (_|  __/\__ \__ \ | (_) | |_| | |_| |_) | |_| | |_                        */              
    /* | | | | | |_) | |_| | |_   | .__/|_|  \___/ \___\___||___/___/  \___/ \__,_|\__| .__/ \__,_|\__|                       */              
    /* |_|_| |_| .__/ \__,_|\__|  |_|                                                 |_|                                     */              
    /*                                                    .d:\utl\sd1                                                         */              
    /*  c: is root for global folder                     / |                                                                  */              
    /*  d: development environment                        /  \ No output table                                                */              
    /*                                                 /                                                                      */              
    /* d:\utl\sas<-DEVELOPMENT FOLDER===.             /     d:--->d:\utl\log                                                  */              
    /*   |                               \           /     /      |                                                           */              
    /*   \utl_intersectWords.sas          \         /     /       \ utl_intersectWords.log                                    */              
    /*                                     \       /     /                                                                    */              
    /* d:\utl\sd1<--TEST CASE DATA------.   \ ____/_    / d:---->d:\utl\lst                                                   */              
    /*   |                               \   /       \ / /       |                                                            */              
    /*  .\utl_intersectWordsTS1.sas7bdat  \ |         | /        \utl_intersectWords.lst                                      */              
    /*                                     -| PROCESS |/                                                                      */              
    /* c:\oto  <--GLOBALAUTOCALL FOLDER----_|         |-------->d:\utl\xls---ISSUE LOG                                        */              
    /*   |                                   \_______ /         |                                                             */              
    /*   \utl_intersectWords.sas ------------./  /  |  \         \utl_intersectWords.xls                                      */              
    /*                                          /   |   \                                                                     */              
    /* c:\ver  <--GLOBAL VERSIONING FOLDER_----/    |    .------d:\utl\pdf DETAIL DOCUMENTATION                               */              
    /*   |                                          |           |                                                             */              
    /*   \utl_intersectWords[date].sas              |            \utl_intersectWords.pdf                                      */              
    /*                                              |                                                                         */              
    /*                                              d:-------> First time you run utl_000init it will create these folders    */              
    /*                                                  |                                                                     */              
    /*                                                  d:\utl\b64   -->  Base 64 encoding for binary exports                 */              
    /*                                                  d:\utl\cdm   -->  Common Data Model All meta data Question Answer     */              
    /*                                                  d:\utl\csv   -->  Delimiited files csv, pipe, tab delimited           */              
    /*                                                  d:\utl\fmt   -->  Formats                                             */              
    /*                                                         \utlFmt.sas7bcat                                               */              
    /*                                                  d:\utl\log   -->  Logs                                                */              
    /*                                                  d:\utl\lst   -->  Lists                                               */              
    /*                                                  d:\utl\pdf   -->  SAP Protocol final CSR Standard macro docs          */              
    /*                                                  d:\utl\png   -->  Graphic Output                                      */              
    /*                                                  d:\utl\raw   -->  Raw data from client                                */              
    /*                                                  d:\utl\rtf   -->  RTf reports                                         */              
    /*                                                  d:\utl\sd1   -->  SAS datasets                                        */              
    /*                                                  d:\utl\sdm   -->  SDTM tables                                         */              
    /*                                                  d:\utl\txt   -->  Related text files                                  */              
    /*                                                  d:\utl\usr   -->  SAS profile SAS user libary                         */              
    /*                                                  d:\utl\xls   -->  All excel Files                                     */              
    /*                                                  d:\utl\xml   -->  Define XML                                          */              
    /*                                                  d:\utl\xpt   -->  SAS V5 transport files                              */              
    /*                                                                                                                        */              
    /**************************************************************************************************************************/              
    /*                                                                                                                        */              
    /*  _   _ ___  __ _  __ _  ___                                                                                            */              
    /* | | | / __|/ _` |/ _` |/ _ \                                                                                           */              
    /* | |_| \__ \ (_| | (_| |  __/                                                                                           */              
    /*  \__,_|___/\__,_|\__, |\___|                                                                                           */              
    /*                  |___/                                                                                                 */              
    /*                                                                                                                        */              
    /*  CREATE SAMPLE DATA                                                                                                    */              
    /*                                                                                                                        */              
    /*  libname sd1 ".:/utl/sd1";                                                                                             */              
    /*  data sd1.have;                                                                                                        */              
    /*    informat src trg $200.;                                                                                             */              
    /*    infile cards delimiter=',';                                                                                         */              
    /*    input src trg;                                                                                                      */              
    /*  cards4;                                                                                                               */              
    /*  one two three four,two three                                                                                          */              
    /*  five six seven eight,one two three four                                                                               */              
    /*  one two three four five,five                                                                                          */              
    /*  roger suzie roger suzie roger,roger roger suzie suzie roger                                                           */              
    /*  sas wps,sas wps                                                                                                       */              
    /*  taco taco taco,taco                                                                                                   */              
    /*  taco,taco taco                                                                                                        */              
    /*  ;;;;                                                                                                                  */              
    /*  run;quit;                                                                                                             */              
    /*                                                                                                                        */              
    /*                                                                                                                        */              
    /*  ARGUMENTS ;                                                                                                           */              
    /*     src     ==>  source list of words                                                                                  */              
    /*     trg     ==>  target list of words                                                                                  */              
    /*     result  ==>  common words                                                                                          */              
    /*                                                                                                                        */              
    /*  %utl_intersectWords(src,trg,result)                                                                                   */              
    /*                                                                                                                        */              
    /*  SAMPLE CALL                                                                                                           */              
    /*                                                                                                                        */              
    /*  data want_macro;                                                                                                      */              
    /*    set sd1.have;                                                                                                       */              
    /*    %utl_intersectWords(                                                                                                */              
    /*       src                                                                                                              */              
    /*      ,trg                                                                                                              */              
    /*      ,result                                                                                                           */              
    /*  )                                                                                                                     */              
    /*  run;quit;                                                                                                             */              
    /*                       _ _                                                                                              */              
    /*   _ __ ___  ___ _   _| | |_                                                                                            */              
    /*  | `__/ _ \/ __| | | | | __|                                                                                           */              
    /*  | | |  __/\__ \ |_| | | |_                                                                                            */              
    /*  |_|  \___||___/\__,_|_|\__|                                                                                           */              
    /*                                                                      COMMON WORDS                                      */              
    /*    SRC                              TRG                              RESULT                                            */              
    /*                                                                                                                        */              
    /*    one two three four               two three                        two three                                         */              
    /*    five six seven eight             one two three four                                                                 */              
    /*    one two three four five          five                             five                                              */              
    /*    roger suzie roger suzie roger    roger roger suzie suzie roger    roger suzie                                       */              
    /*    sas wps                          sas wps                          sas wps                                           */              
    /*    taco taco taco                   taco                             taco                                              */              
    /*    taco                             taco taco                        taco                                              */              
    /*                                                                                                                        */              
    /**************************************************************************************************************************/              
                                                                                                                                              
    %macro utl_intersectWords(                                                                                                                
        arg_src      /* source list of words */                                                                                               
       ,arg_trg      /* target list of words */                                                                                               
       ,arg_result   /* common words         */                                                                                               
       ) /des= "Create a list of distinct common words contained in two lists";                                                               
                                                                                                                                              
       %local res ;                                                                                                                           
                                                                                                                                              
       /*--- check arguments                                                         --*/                                                     
                                                                                                                                              
       put ;                                                                                                                                  
       put " %sysfunc(ifc(%sysevalf(%superq(arg_src)=,boolean),    **** Please Provide blank separated source word list ****,))" ;            
       put " %sysfunc(ifc(%sysevalf(%superq(arg_trg)=,boolean),    **** Please Provide blank separated target word list ****,))" ;            
       put " %sysfunc(ifc(%sysevalf(%superq(arg_result)=,boolean) ,**** Please Provide return variable                  ****,))" ;            
                                                                                                                                              
        %let res= %eval                                                                                                                       
        (                                                                                                                                     
            %sysfunc(ifc(%sysevalf(%superq(arg_src    )=,boolean),1,0))                                                                       
          + %sysfunc(ifc(%sysevalf(%superq(arg_trg    )=,boolean),1,0))                                                                       
          + %sysfunc(ifc(%sysevalf(%superq(arg_result )=,boolean),1,0))                                                                       
        ) ;                                                                                                                                   
                                                                                                                                              
        %put &=res;                                                                                                                           
                                                                                                                                              
         /*-- missing one or more arguments                                          --*/                                                     
                                                                                                                                              
         %if &res ne 0 %then %do ;                                                                                                            
                                                                                                                                              
           put ;                                                                                                                              
           put '+---------------------------------------------+';                                                                             
           put '|                                             |';                                                                             
           put '| Sample Call                                 |';                                                                             
           put '|                                             |';                                                                             
           put '| %utl_intersectWords(src,trg,result)         |';                                                                             
           put '|                                             |';                                                                             
           put '| %utl_intersectWords(                        |';                                                                             
           put '|    src     ==>  source list of words        |';                                                                             
           put '|   ,trg     ==>  target list of words        |';                                                                             
           put '|   ,result  ==>  common words                |';                                                                             
           put '| )                                           |';                                                                             
           put '|                                             |';                                                                             
           put '+---------------------------------------------+';                                                                             
           put ;                                                                                                                              
                                                                                                                                              
           put 'ERROR: macro utl_intersectWords stopped due to incorrect invocation';                                                         
                                                                                                                                              
           stop;                                                                                                                              
                                                                                                                                              
           run;quit;                                                                                                                          
                                                                                                                                              
        %end;                                                                                                                                 
                                                                                                                                              
        /*-- all arguments ok                                                        --*/                                                     
        /*-- start common variable code                                              --*/                                                     
        %else %do;                                                                                                                            
                                                                                                                                              
            /*-- none of the variables created by this macro will be in the output   --*/                                                     
            /*-- temporary arrays are not written to output datasets                 --*/                                                     
                                                                                                                                              
            array __c[2] $ 200 _temporary_;                                                                                                   
            array __n[1] 8     _temporary_;  /*-- for saving _n_                     --*/                                                     
                                                                                                                                              
            /*-- unfortunately the do statement does not support _temporary_ arrays  --*/                                                     
            /*-- save currenr temprary _n_ value to retore later                     --*/                                                     
            __n[1] = _n_;                                                                                                                     
                                                                                                                                              
            /*--  hold _temporary_ intermediate values                                                                                        
               __c[1] temp storage for single words from source                                                                               
               __c[2] remp storage for intermediate results                                                                                   
            --*/                                                                                                                              
                                                                                                                                              
            /*-- cannot use a temp variable in do statement                          --*/                                                     
            /*-- iterate over each word in source string                             --*/                                                     
                                                                                                                                              
            do  _n_ = 1 to  countw(&arg_src)   ;                                                                                              
                                                                                                                                              
                __c[1] = scan(&arg_src,_n_) ; /*-- get each word in the source list  --*/                                                     
                                                                                                                                              
                /*-- if the source word in target string then proceed                --*/                                                     
                if index(&arg_trg,strip(__c[1])) > 0 then do ;                                                                                
                                                                                                                                              
                     /* -- do not add word to result list if already present         --*/                                                     
                     if index(__c[2],strip(__c[1])) = 0 then                                                                                  
                        __c[2]=catx(" ",__c[2],__c[1]);                                                                                       
                                                                                                                                              
                end ; /* adding one word that is not aready in result list           --*/                                                     
                                                                                                                                              
            end; /* end adding distinct words in source and target to result list    --*/                                                     
                                                                                                                                              
            &arg_result      = __c[2] ;                                                                                                       
                                                                                                                                              
            __c[2] = "";                                                                                                                      
                                                                                                                                              
            * restore _n_; /*-- restore original bact to calling program             --*/                                                     
            _n_ = __n[1] ;                                                                                                                    
                                                                                                                                              
        %end;                                                                                                                                 
                                                                                                                                              
    %mend utl_intersectWords;                                                                                                                 
    ;;;;                                                                                                                                      
    run;quit;                                                                                                                                 
                                                                                                                                              
    /*              _                                                                                                                         
      ___ _ __   __| |                                                                                                                        
     / _ \ `_ \ / _` |                                                                                                                        
    |  __/ | | | (_| |                                                                                                                        
     \___|_| |_|\__,_|                                                                                                                        
                                                                                                                                              
    */                                                                                                                                        
                                                          
