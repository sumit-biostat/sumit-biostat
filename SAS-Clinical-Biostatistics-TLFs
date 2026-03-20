Libname CANCER_L "/home/u64214007/Git/New Folder";

/* STEP :Import Data */
proc import datafile="/home/u64214007/Git/New Folder/survey lung cancer.csv"
     out=CANCER_L.RAW
     dbms=csv
     replace;
     getnames=yes;
run; 

/* STEP 2: Data Cleaning + Labeling */

data CANCER_L.CLEAN;
    set CANCER_L.RAW;

    /* Convert numeric to meaningful labels */
    format smoking yn.
           yellow_fingers yn.
           anxiety yn.
           peer_pressure yn.
           chronic_disease yn.
           fatigue yn.
           allergy yn.
           wheezing yn.;

    /* Create format */
run;

proc format;
    value yn
        1 = "No"
        2 = "Yes";
run;

/* STEP 3: Create Derived Variables */

data  CANCER_L.Analysis;
    set CANCER_L.CLEAN;

    /* Age Group */
    if age < 50 then age_grp = "<50";
    else if age < 65 then age_grp = "50-64";
    else age_grp = "65+";

run;


/* STEP 4: Table 1 Demographics */

proc means data=CANCER_L.Analysis n mean std min max;
    var age;
run;

proc freq data=CANCER_L.Analysis;
    tables gender age_grp / nocum;
run;

/* STEP 5: Table 2 */

proc freq data=CANCER_L.Analysis;
    tables smoking yellow_fingers anxiety peer_pressure 
           chronic_disease fatigue allergy wheezing / nocum;
run;

/* STEP 6: Chi-Square Tests */

proc freq data=CANCER_L.Analysis;
    tables smoking*wheezing / chisq;
run;

proc freq data=CANCER_L.Analysis;
    tables smoking*yellow_fingers / chisq;
run;

/* STEP 7: T-Test */

proc ttest data=CANCER_L.Analysis;
    class smoking;
    var age;
run;

/* STEP 8: ANOVA */

proc anova data=CANCER_L.Analysis;
    class fatigue;
    model age = fatigue;
run;
quit;

/* STEP 9: Correlation */

proc corr data=CANCER_L.Analysis;
    var age smoking fatigue anxiety;
run;

/* STEP 10: Regression */

proc reg data=CANCER_L.Analysis;
    model age = smoking fatigue anxiety;
run;
quit;

/* STEP 11: TLF OUTPUT */
/* Tables (PDF) */

ods pdf file="/home/u64214007/Git/New Folder/output\TLF_Report.pdf";

title "Table 1: Demographics";

proc means data=CANCER_L.Analysis mean std;
    var age;
run;

proc freq data=CANCER_L.Analysis;
    tables gender age_grp;
run;

ods pdf close;

/* Listings */

proc print data=CANCER_L.Analysis (obs=20);
run;

/* Figures (Graphs) */

proc sgplot data=CANCER_L.Analysis;
    histogram age;
run;

proc sgplot data=CANCER_L.Analysis;
    vbar smoking;
run;

proc sgplot data=CANCER_L.Analysis;
    vbox age / category=smoking;
run;

ods rtf file="/home/u64214007/Git/New Folder/output\Clinical_TLF_Report.rtf" style=journal;

title "Table 1: Demographics";

proc means data=CANCER_L.Analysis mean std;
    var age;
run;

proc freq data=CANCER_L.Analysis;
    tables gender smoking;
run;

ods rtf close;











