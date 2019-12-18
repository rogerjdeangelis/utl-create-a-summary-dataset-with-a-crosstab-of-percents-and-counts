# utl-create-a-summary-dataset-with-a-crosstab-of-percents-and-counts
Create a summary dataset with a crosstab of percents and counts
    SAS Forum: Create a summary dataset with a crosstab of percents and counts

    Unlike proc tablulate you create a dataset not a static report

    github
    https://tinyurl.com/svlc5j2
    https://github.com/rogerjdeangelis/utl-create-a-summary-dataset-with-a-crosstab-of-percents-and-counts

    SAS Forum
    https://tinyurl.com/qn5arwr
    https://communities.sas.com/t5/New-SAS-User/creating-summary-table-in-SAS/m-p/610613


    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    data have;
    input ID Month $ Points;
    cards;
    1 Jan 100
    2 Jan 100
    3 Jan 200
    4 Feb 100
    5 Feb 200
    6 Feb 300
    7 Mar 100
    8 Mar 200
    9 Apr 300
    10 Apr 100
    11 Apr 200
    12 Apr 300
    ;
    run;

     WORK.HAVE total obs=12

      ID    MONTH    POINTS

       1     Jan       100
       2     Jan       100
       3     Jan       200
       4     Feb       100
       5     Feb       200
       6     Feb       300
       7     Mar       100
       8     Mar       200
       9     Apr       300
      10     Apr       100
      11     Apr       200
      12     Apr       300

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;


    WORK.WANT total obs=5

      LABEL      _100       _200       _300     SUM

       Apr     25.0000    25.0000    50.0000      4
       Feb     33.3333    33.3333    33.3333      3
       Jan     66.6667    33.3333     0.0000      3
       Mar     50.0000    50.0000     0.0000      2
       Sum      5.0000     4.0000     3.0000     12

     *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    ods exclude all;
    ods output rowprofilespct=havpct ;
    ods output observed=havcnt;
    proc corresp data=have dim=1 observed missing all print=both;;
    tables month, points;
    run;quit;
    ods select all;

    data want;
      merge havcnt havpct;
    run;quit;


