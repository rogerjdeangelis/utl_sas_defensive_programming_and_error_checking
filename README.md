# utl_sas_defensive_programming_and_error_checking
SAS defensive programming and error checking Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    SAS defensive programming and error checking

     Below are some techniques for defensive programming

      1. SAS error handling options
      2. Using text 'ERROR" but preventing log checkers from flaging your code when an error did not occur.
      3. Definition of return codes
      4. Color user generated errors in read and warnings in green
      5. Note2err forcing the most serious sas notes to be errors
      6. Checking macro arguments Chang Chung
      7. Problematic text in notes, warnings or errors
      8. Defensive select/when statement
      9. Parsing sysinfo from proc compare
     10. Turn everthing off so that only put statements appear in log - useful to turn all off then turn on just what you want
     11. Turn everthing on good for debugging
     12. Using syserr and sysmsg along with errorcheck=strict to check completion status
     13. Messages on/off Tom Abernathy
     14. Run cancel


    *                           _                     _ _ _
      ___ _ __ _ __ ___  _ __  | |__   __ _ _ __   __| | (_)_ __   __ _
     / _ \ '__| '__/ _ \| '__| | '_ \ / _` | '_ \ / _` | | | '_ \ / _` |
    |  __/ |  | | | (_) | |    | | | | (_| | | | | (_| | | | | | | (_| |
     \___|_|  |_|  \___/|_|    |_| |_|\__,_|_| |_|\__,_|_|_|_| |_|\__, |
                                                                  |___/
    ;

    proc options group=errorhandling;
    run;quit;


     AUTOCORRECT       Automatically corrects misspelled procedure
                       names and keywords, and global statement names.
     BYERR             SAS issues an error message and stops processing
                       if the SORT procedure attempts to sort a _NULL_ data set.
     NOCHKPTCLEAN      Does not erase files in the Work library after a
                       batch program successfully executes in checkpoint mode or restart mode.
     CLEANUP           Performs automatic continuous cleanup of
                       non-essential resources in out-of-resource conditions.
     DKRICOND=WARN     Specifies the error level to report when a variable
                       is missing from an input data set during the processing of a
                        DROP=, KEEP=, or RENAME= data set
                       option.
     DKROCOND=WARN     Specifies the error level to report when a variable is missing from
                       an output data set during the processing of a DROP=, KEEP=, or RENAME= data set
                       option.
     NODMSSYNCHK       Disables syntax check mode for DATA step and
                       PROC step processing in the windowing environment.
     DS2SCOND=WARN     Specifies the type of message that PROC DS2 generates.
     DSNFERR           Issues an error message and stops processing
                       when a SAS data set cannot be found.
     NOERRORABEND      Does not end SAS for most errors, issues an
                       error message, sets OBS=0, and goes into syntax check mode.
     NOERRORBYABEND    Does not end a SAS program when an error
                       occurs in BY-group processing, issues an error, and continues processing.
     ERRORCHECK=NORMAL Specifies whether SAS enters syntax-check mode when
                       errors are found in the LIBNAME, FILENAME,  %INCLUDE, and LOCK statements.
     ERRORS=20         Specifies the maximum number of observations for which
                       SAS issues complete error messages.
     NOFMTERR          Issues a note for missing variable formats, uses w. or
                       $w., and continues processing.
     NOLABELCHKPT      For batch programs, disables the recording of
                       checkpoint-restart data for labeled code sections.
     LABELCHKPTLIB=WORK
                       Specifies the libref of the library where the
                       checkpoint-restart data is saved for labeled code sections.
     NOLABELRESTART    Disables restart mode, which executes batch programs
                       using checkpoint-restart data collected at labeled code sections.
     NOQUOTELENMAX     Does not write a warning message to the SAS log if a
                       quoted string exceeds the maximum length allowed.
     NOSTEPCHKPT       Disables recording of checkpoint-restart data for
                       DATA and PROC steps for batch programs.
     STEPCHKPTLIB=WORK Specifies the libref of the library where
                       checkpoint-restart data for DATA and PROC steps is saved.
     NOSTEPRESTART     Disables restart mode which executes batch programs
                       using checkpoint-restart data collected for
                       DATA and PROC steps in a prior execution.
     SYNTAXCHECK       Enables syntax check mode for multiple steps in
                       non-interactive or batch SAS sessions.
     VNFERR            SAS issues an error message when a BY variable
                       exists in one data set but not another when the other data set is _NULL_.


    *                               _     _                           _
     _ __  _ __ _____   _____ _ __ | |_  | | ___   __ _  ___ ___  ___| | __
    | '_ \| '__/ _ \ \ / / _ \ '_ \| __| | |/ _ \ / _` |/ __/ _ \/ __| |/ /
    | |_) | | |  __/\ V /  __/ | | | |_  | | (_) | (_| | (_|  __/ (__|   <
    | .__/|_|  \___| \_/ \___|_| |_|\__| |_|\___/ \__, |\___\___|\___|_|\_\
    |_|                                           |___/
    ;

    data _null_;

      set sashelp.class;
      if sex='X' then put "%str(ERR)OR sex is incorrect";

    run;quit;


    *         _                                    _
     _ __ ___| |_ _   _ _ __ _ __     ___ ___   __| | ___  ___
    | '__/ _ \ __| | | | '__| '_ \   / __/ _ \ / _` |/ _ \/ __|
    | | |  __/ |_| |_| | |  | | | | | (_| (_) | (_| |  __/\__ \
    |_|  \___|\__|\__,_|_|  |_| |_|  \___\___/ \__,_|\___||___/

    ;

    SAS error and warning codes

    proc format;
       value status_code
          .           = "Program was not run"
          low -< 0    = "SAS Failed to Initialize"
          0           = "SAS Ended Successfully"
          1           = "SAS Ended With Warnings"
          2           = "SAS Ended With Errors"
          3           = "User issued the ABORT statement"
          4           = "User issued the ABORT RETURN statement"
          5           = "User issued the ABORT ABEND statement"
          6           = "SAS internal error"
          >6 - high   = "User specified RETURN code"
       ;
       run;quit


      data NULL;
          set doesnotexist;
       run;
       %put Errortext= &syserrortext;
       %put Errorcode= &syserr;

       Errortext = File WORK.DOESNOTEXIST.DATA does not exist.
       Errorcode = 1012.

    *                                _            _
     _ __ ___  ___  __ _    ___ ___ | | ___  _ __(_)_ __   __ _
    | '_ ` _ \/ __|/ _` |  / __/ _ \| |/ _ \| '__| | '_ \ / _` |
    | | | | | \__ \ (_| | | (_| (_) | | (_) | |  | | | | | (_| |
    |_| |_| |_|___/\__, |  \___\___/|_|\___/|_|  |_|_| |_|\__, |
                   |___/                                  |___/
    ;

    * change the color of user generated warning and error messages;
      data _null_;
          put 'ERROR: This is red';
          put 'WARNING: This is Green';
       run;
    *            _       ____
     _ __   ___ | |_ ___|___ \ ___ _ __ _ __
    | '_ \ / _ \| __/ _ \ __) / _ \ '__| '__|
    | | | | (_) | ||  __// __/  __/ |  | |
    |_| |_|\___/ \__\___|_____\___|_|  |_|

    ;

    T002230 NOTE2ERR FORCING THE MOST SERIOUS SAS NOTES TO BE ERRORS

       options dsoptions="note2err";
         data tst;
           length agechr $8 num 8.;
           set sashelp.class;
           num='1';
           agechr=age;
         run;

       22315  options dsoptions="note2err";
       22316    data tst;
       22317      length agechr $8 num 8.;
       22318      set sashelp.class;
       22319      num='1';
       ERROR: Character value found where numeric value needed at line 22319 column 9.
       22320      agechr=age;
       ERROR: Numeric value found where character value needed at line 22320 column 12.
       22321    run;

       Here is the list of the messages that get turned into ERROR messages.

        19  Variable %*v is uninitialized.
        97  Missing values were generated as a result of
        98  Division by zero detected at %2q.
        99  Mathematical operations could not be performed
       108  Invalid numeric data, '%*s' , at %2q.
       109  Invalid character data, %f , at %2q.
       110  Invalid %sargument to function %b at %2q.
       139  Argument to function %*s is not a known variable name:  %*v.
       140  Argument to function %*s is not a valid variable name:  %*s.
       205  Invalid argument(s) to the exponential operator "**" at %2q.
       208  Invalid numeric data, %*s='%*s' , at %2q.
       209  Invalid character data, %*s=%f , at %2q.
       223  A number has become too large at %2q. %w%*s
       224  A number has become too large during the compilation phase.
       225  Division by zero detected during the compilation phase.
       242  Invalid argument(s) to the exponential operator "**".
       258  Invalid argument to function %*b at %2q.
       259  Invalid first argument to function %*b at %2q.
       260  Invalid second argument to function %*b at %2q.
       261  Invalid third argument to function %*b at %2q.
       262  Invalid fourth argument to function %*b at %2q.
       267  Argument %d to function %*b at %2q is invalid.
       356  The SUBSTR pseudo-variable function does not allow character
       424  Character values have been converted to numeric
       425  Numeric values have been converted to character
       429  A number has become too large during the compilation phase,
       430  Division by zero detected during the compilation phase,
       484  Format %*b was not found or could not be loaded.
       485  Informat %*b was not found or could not be loaded.

       The numbers are the traditional obscure SAS error codes which
       are printed in the logs so that people will scratch their heads
       a lot.

    *     _               _
      ___| |__   ___  ___| | __   __ _ _ __ __ _ ___
     / __| '_ \ / _ \/ __| |/ /  / _` | '__/ _` / __|
    | (__| | | |  __/ (__|   <  | (_| | | | (_| \__ \
     \___|_| |_|\___|\___|_|\_\  \__,_|_|  \__, |___/
                                           |___/
    ;

    /* T000520 CHECKING MACRO ARGUMENTS CHANG CHUNG
      %macro checker(sysbin=,outdsn=);

       %put %sysfunc(ifc(%sysevalf(%superq(sysbin )=,boolean),**** Please Provide the sysinfo value    ****,));
       %put %sysfunc(ifc(%sysevalf(%superq(outdsn )=,boolean),**** Please Provide an output dataset    ****,));

        %let res= %eval
        (
            %sysfunc(ifc(%sysevalf(%superq(sysbin )=,boolean),1,0))
          + %sysfunc(ifc(%sysevalf(%superq(outdsn )=,boolean),1,0))
        );

         %if &res = 0 %then %do;
             %put do some work;
         %end;

      %mend checker;

      %checker;
      Log


    *                           _            _
      ___ _ __ _ __ ___  _ __  | |_ _____  _| |_
     / _ \ '__| '__/ _ \| '__| | __/ _ \ \/ / __|
    |  __/ |  | | | (_) | |    | ||  __/>  <| |_
     \___|_|  |_|  \___/|_|     \__\___/_/\_\\__|

    ;

    Problematic text in notes, warnings or errora

        Array Notes{328} $64 (
                " 0 observations rewritten, 0 observations added and 0 observations deleted"
                " 0 lines"
                " 0 observations"
                " 0 records"
                " 0 rows"
                " 0 obs"
                "%to value of"
                "traceback"
                "not currently in"
                "abnormally terminated"
                "access denied"
                "allowed"
                "already been defined"
                "ambiguous"
                "apparent invocation"
                "apparent symbolic"
                "appears as text"
                "are not allowed"
                "argument 1"
                "argument 2"
                "argument 3"
                "assumed"
                "assuming"
                "at least"
                "but appears on"
                "but is not"
                "but no"
                "by-group"
                "cannot be accessed"
                "cannot be deleted"
                "cannot be found"
                "cannot be loaded"
                "cannot be"
                "cannot open"
                "cartesian"
                "central parameter"
                "character in one"
                "clause has been augmented"
                "clause references"
                "cli error"
                "closing"
                "columns were too wide"
                "condition in the"
                "conflicting attributes"
                "contain no data in common"
                "contain unequal"
                "contains 1 variable"
                "convert"
                "converted to"
                "converted"
                "could not be fit"
                "could not be found"
                "could not be loaded"
                "could not be written"
                "could not be"
                "could not"
                "creates a common"
                "default estimation"
                "default style will be used instead"
                "defined as both"
                "did not satisfy"
                "differ"
                "different data types"
                "division by zero"
                "denied"
                "does not exist"
                "does not have enough arguments"
                "does not match"
                "doesn't appear"
                "doesn't have"
                "double-dash"
                "due to a"
                "due to errors"
                "due to looping"
                "dummy macro"
                "ellipsoid centered"
                "end of macro"
                "end-of-record"
                "ending execution"
                "enter run"
                "error"
                "errorabend"
                "errorcheck=strict"
                "errors noted"
                "estimated autoregression parameter"
                "examine fields"
                "exceed"
                "exceeds 8 characters"
                "execution terminated"
                "execution terminating"
                "expected"
                "expecting"
                "experimental release"
                "export cancelled"
                "expression has no"
                "extraneous information"
                "extraneous"
                "failed to converge"
                "failed to load"
                "failed to"
                "fatal"
                "firstobs option >"
                "firstobs option"
                "generic critical"
                "groups are not created"
                "hanging"
                "has 0 observations"
                "has _type_"
                "has a null"
                "has already been defined"
                "has already been set"
                "has already been"
                "has already"
                "has become more"
                "has been reduced"
                "has been transformed"
                "has been discarded"
                "has changed"
                "has different lengths"
                "has exceeded"
                "has multiple"
                "has never been"
                "has no condition"
                "has not been dropped"
                "has not been"
                "has too few"
                "have been detected"
                "ignored"
                "ignoring"
                "illegal"
                "included in the"
                "incomplete"
                "incorrect"
                "input data set is empty"
                "input distances have been squared"
                "input data set is already sorted"
                "input empty"
                "insufficient"
                "invalid"
                "is already on the"
                "is already sorted"
                "is also a dataset"
                "is ambiguous"
                "is before the starting"
                "is included"
                "is invalid"
                "is less than or equal"
                "is less than"
                "is not a valid"
                "is not allowed"
                "is not assigned"
                "is not greater than"
                "is not in effect"
                "is not in"
                "is not on file"
                "is not recognized"
                "is not sorted"
                "is not valid"
                "is obsolete"
                "is sequential"
                "it is used out"
                "is obsolete"
                "labels differ"
                "length of numeric"
                "limit set by"
                "list empty"
                "lost card"
                "mathemat"
                "mathematical operations"
                "may be incomplete"
                "may be longer"
                "may not be as expected"
                "merge statement has more than"
                "merge statement"
                "missing close parentheses"
                "missing equals sign"
                "missing on every"
                "missing semicolon"
                "missing values"
                "missing"
                "misspelled"
                "mixed engine types"
                "model contains"
                "more positional"
                "multiple lengths"
                "multiple optimal"
                "multiple vertical"
                "multiple"
                "must be character"
                "must be entered"
                "must be followed"
                "must be given"
                "must be invoked"
                "must have appeared"
                "must precede"
                "no body file"
                "no body"
                "no by"
                "no cards"
                "no data set"
                "no data sets qualify"
                "no data"
                "no effect"
                "no expression"
                "no file"
                "no formats found"
                "no keep"
                "no libraries"
                "suppress"
                "no logical"
                "no longer exists"
                "no matching"
                "no non-missing"
                "no observations"
                "no output destinations active"
                "no output produced"
                "no output"
                "no rows were selected"
                "no rows"
                "no shape"
                "no variables found"
                "no variables specified"
                "no variables"
                "none of the options"
                "not a valid"
                "not acceptable"
                "not adjusted"
                "not all"
                "not allow character"
                "not be included"
                "not be performed"
                "not currently licensed"
                "not found"
                "not in a valid"
                "not in effect"
                "not licensed"
                "not on input data set"
                "not processed"
                "not recognized"
                "not replaced because"
                "not resolved"
                "not valid for import"
                "not valid"
                "not written"
                "null where"
                "numeric in one"
                "obs=0"
                "observations not"
                "obsolete"
                " omitted due to"
                "occurred on module"
                "offline"
                "one or more lines may be longer"
                "operand was found in"
                "option value"
                "outside the axis"
                "parenthesis for"
                "previous errors"
                "proc sql statements are executed immediately"
                "product not found"
                "product with which"
                "partial initialization"
                "quoted string"
                "recursion"
                "recursive"
                "reference"
                "references the data set being updated"
                "refers to the same"
                "refers to"
                "repeat"
                "request ignored"
                "required operator not found"
                "requires a numeric"
                "requires compatible"
                "right-hand"
                "sas set option obs=0"
                "sas went"
                "scale parameter was held fixed"
                "shifted"
                "starting variable"
                "statement has been deleted"
                "statement needs"
                "statement syntax"
                "statement transforms"
                "statistics are mean corrected"
                "statistics requested"
                "stop"
                "stopped"
                "subroutine"
                "syntax for this"
                "the chi-square is small"
                "too long"
                "too many array subscripts"
                "too small"
                "too wide"
                "trying to use"
                "unable to initialize"
                "unable to"
                "unable"
                "unbalanced"
                "unclosed"
                "undeclared"
                "undetermined"
                "uninitialized"
                "unknown"
                "unrecognized"
                "unref"
                "unresolved"
                "unsatisfied"
                "violation"
                "was disabled"
                "was stopped"
                "was misspelled as"
                "was not created"
                "was not defined"
                "was not found"
                "was not replaced"
                "went to a new line"
                "were excluded"
                "will be assumed"
                "will be used"
                "will not be"
                "will not load"
                "will stop executing"
                "within the range"
                "you can only"
                "your request"
                "zero elements" );
              %mend ary;

    *    _       __                _              _           _
      __| | ___ / _| ___ _ __   __| |    ___  ___| | ___  ___| |_
     / _` |/ _ \ |_ / _ \ '_ \ / _` |   / __|/ _ \ |/ _ \/ __| __|
    | (_| |  __/  _|  __/ | | | (_| |   \__ \  __/ |  __/ (__| |_
     \__,_|\___|_|  \___|_| |_|\__,_|   |___/\___|_|\___|\___|\__|

    ;

     /* T002130 DEFENSIVE PROGRAMMING - LEAVE OFF THE OTHERWISE IN A SELECT CLAUSE TO FORCE AN ERROR */
       data sample;
          do grp=1 to 3;
            output;
          end;
       run;
       data err;
         set sample;
         select (grp);
            when (1) put grp=;
            when (2) put grp=;
         end;
       run;
       GRP=1
       GRP=2
       ERROR: Unsatisfied WHEN clause and no OTHERWISE clause at line 22687 column 6.


    *
      ___ ___  _ __ ___  _ __   __ _ _ __ ___
     / __/ _ \| '_ ` _ \| '_ \ / _` | '__/ _ \
    | (_| (_) | | | | | | |_) | (_| | | |  __/
     \___\___/|_| |_| |_| .__/ \__,_|_|  \___|
                        |_|
    ;

    /* T000190 PARSING SYSINFO FROM PROC COMPARE
        data sysnfo;sysinfo=&sysinfo;length msg $64;
           if sysnfo =0                   then do; /* 0 */ msg = 'All Compared Variables Equal'; output; end;
           if sysinfo='...............1'b then do; /* 1 */ msg = 'Data set labels differ'; output; end;
           if sysinfo='..............1.'b then do; /* 2 */ msg = 'Data set types differ'; output; end;
           if sysinfo='.............1..'b then do; /* 3 */ msg = 'Variable has different informat'; output; end;
           if sysinfo='............1...'b then do; /* 4 */ msg = 'Variable has different format'; output; end;
           if sysinfo='...........1....'b then do; /* 5 */ msg = 'Variable has different length'; output; end;
           if sysinfo='..........1.....'b then do; /* 6 */ msg = 'Variable has different label'; output; end;
           if sysinfo='.........1......'b then do; /* 7 */ msg = 'Base data set has observation not in comparison'; output; end;
           if sysinfo='........1.......'b then do; /* 8 */ msg = 'Comparison data set has observation not in base'; output; end;
           if sysinfo='.......1........'b then do; /* 9 */ msg = 'Base data set has BY group not in comparison '; output; end;
           if sysinfo='......1.........'b then do; /* 10*/ msg = 'Comparison data set has BY group not in base '; output; end;
           if sysinfo='.....1..........'b then do; /* 11*/ msg = 'Base data set has variable not in comparison '; output; end;
           if sysinfo='....1...........'b then do; /* 12*/ msg = 'Comparison data set has variable not in base '; output; end;
           if sysinfo='...1............'b then do; /* 13*/ msg = 'A value comparison was unequal '; output; end;
           if sysinfo='..1.............'b then do; /* 14*/ msg = 'Conflicting variable types '; output; end;
           if sysinfo='.1..............'b then do; /* 15*/ msg = 'BY variables do not match '; output; end;
           if sysinfo='1...............'b then do; /* 16*/ msg = 'Fatal error: comparison not done '; output; end;
        run;
        proc print data=sysnfo;;run;


     /* T000470 TURN EVERTHING OFF SO THAT ONLY PUT STATEMENTS APPEAR IN LOG - USEFUL TO TURN ALL OFF THEN TURN ON JUST WHAT YOU WANT */
    %macro utl_nopts
        / des = "Turn all debugging options off";
         TITLE;
         FOOTNOTE;
      options
         PAGENO=1
         NOQUOTELENMAX
         ERROR=1
         VALIDVARNAME=UPCASE
         NOSOURCE     /* turn sas source statements off                   */
         NOFMTERR     /* turn  Format Error off                           */
         NOMACROGEN   /* turn  MACROGENERATON off                         */
         NOSYMBOLGEN  /* turn  SYMBOLGENERATION off                       */
         NONOTES      /* turn  NOTES off                                  */
         NOOVP        /* never overstike                                  */
         NOCMDMAC     /* turn  CMDMAC command macros on                   */
         NOSOURCE2    /* turn  SOURCE2   show gererated source off        */
         NOMLOGIC     /* turn  MLOGIC    macro logic off                  */
         NOMPRINT     /* turn  MPRINT    macro statements off             */
         NOCENTER     /* turn  NOCENTER  I do not like centering          */
         NOMTRACE     /* turn  MTRACE    macro tracing                    */
         NOSERROR     /* turn  SERROR    show unresolved macro refs       */
         NOMERROR     /* turn  MERROR    show macro errors                */
         OBS=MAX      /* turn  max obs on                                 */
         NOFULLSTIMER /* turn  FULLSTIMER  give me all space/time stats   */
         NODATE       /* turn  NODATE      suppress date                  */
         MERGENOBY=NOWARN
         NOERRORABEND
         NOERRORBYABEND
         NONUMBER
         NONOTES
         NOSOURCE2
         SYSRPUTSYNC
         /* NO$SYNTAXCHECK  be careful with this one */
    ;
    run;
    %mend utl_nopts;

    /* T000480 TURN EVERTHING ON GOOD FOR DEBUGGING */
    %macro utl_opts;
    options
         MAUTOLOCDISPLAY  /* DISPLAY LOCATION OF AUTOCALL MACROS */
         VALIDVARNAME=UPCASE
         QUOTELENMAX
         OBS=MAX
         NOFMTERR    /* DO NOT FAIL ON MISSING FORMATS                              */
         SOURCE      /* turn sas source statements on                               */
         MACROGEN    /* turn  MACROGENERATON ON  deprecated?                        */
         SYMBOLGEN   /* turn  SYMBOLGENERATION ON                                   */
         NOTES       /* turn  NOTES ON                                              */
         NOOVP       /* never overstike                                             */
         CMDMAC      /* turn  CMDMAC command macros on                              */
         ERRORS=2    /* turn  ERRORS=2  max of two errors                           */
         SOURCE2     /* turn  SOURCE2   show gererated source ( included source )   */
         MLOGIC      /* turn  MLOGIC    macro logic                                 */
         MPRINT      /* turn  MPRINT    macro statements                            */
         MRECALL     /* turn  MRECALL   always recall                               */
         MERROR      /* turn  MERROR    show macro errors                           */
         NOCENTER    /* turn  NOCENTER  I do not like centering                     */
         DETAILS     /* turn  DETAILS   show details in dir window                  */
         SERROR      /* turn  SERROR    show unresolved macro refs                  */
         NONUMBER    /* turn  NONUMBER  do not number pages                         */
         FULLSTIMER  /* turn  FULLSTIMER  give me all space/time stats              */
         NODATE      /* turn  NODATE      suppress date                             */
         NOTES
         SPOOL
     ;
    run;
    %mend utl_opts;
    *               _
      ___ _   _ ___| |_ ___  _ __ ___
     / __| | | / __| __/ _ \| '_ ` _ \
    | (__| |_| \__ \ || (_) | | | | | |
     \___|\__,_|___/\__\___/|_| |_| |_|

    ;
    /* T003990 USING SYSERR and SYSMSG ALONG WITH ERRORCHECK=STRICT TO CHECK COMPLETION STATUS
    options fmterr errorcheck=strict;
    data err;
      format name $qxzr.; /* non existent format */
      set sashelp.class;
    run;
    %sysfunc(ifc(&syserr=0,%nrstr(%put all formats found;),%nrstr(%put ***** Demog Format %str(ER)ROR - Code &syserr &sysmsg ********;))

    *                                             __  __
     _ __ ___  ___  __ _     ___  _ __      ___  / _|/ _|
    | '_ ` _ \/ __|/ _` |   / _ \| '_ \    / _ \| |_| |_
    | | | | | \__ \ (_| |  | (_) | | | |  | (_) |  _|  _|
    |_| |_| |_|___/\__, |   \___/|_| |_|   \___/|_| |_|
                   |___/
    ;

    %let messages=*;  /* turn messages off */
    %let messages=;   /* turn messages on  */

    %put Tom Abernathy message on/off capability;
    data _null_;
        call symputx('message_d','&messages.putlog');  /* Use &message_d inside of data steps for execution messages */
        call symputx('message_m','%&messages.put');  /* Use &message_m for compile time messages */
      run;

    *                                             _
     _ __ _   _ _ __      ___ __ _ _ __   ___ ___| |
    | '__| | | | '_ \    / __/ _` | '_ \ / __/ _ \ |
    | |  | |_| | | | |  | (_| (_| | | | | (_|  __/ |
    |_|   \__,_|_| |_|   \___\__,_|_| |_|\___\___|_|

    ;

    proc freq data=school;
    table gender * ethnicity;
    run cancel;


