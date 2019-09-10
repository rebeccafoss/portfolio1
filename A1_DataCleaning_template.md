Assignment 1, Language development in Autism Spectrum Disorder (ASD) - Brushing up your code skills
===================================================================================================

In this first part of the assignment we will brush up your programming
skills, and make you familiar with the data sets you will be analysing
for the next parts of the assignment.

In this warm-up assignment you will: 1) Create a Github (or gitlab)
account, link it to your RStudio, and create a new repository/project 2)
Use small nifty lines of code to transform several data sets into just
one. The final data set will contain only the variables that are needed
for the analysis in the next parts of the assignment 3) Warm up your
tidyverse skills (especially the sub-packages stringr and dplyr), which
you will find handy for later assignments.

N.B: Usually you’ll also have to doc/pdf with a text. Not for Assignment
1.

Learning objectives:
--------------------

-   Become comfortable with tidyverse (and R in general)
-   Test out the git integration with RStudio
-   Build expertise in data wrangling (which will be used in future
    assignments)

0. First an introduction on the data
------------------------------------

Language development in Autism Spectrum Disorder (ASD)
======================================================

Reference to the study:
<a href="https://www.ncbi.nlm.nih.gov/pubmed/30396129" class="uri">https://www.ncbi.nlm.nih.gov/pubmed/30396129</a>

Background: Autism Spectrum Disorder (ASD) is often related to language
impairment, and language impairment strongly affects the patients
ability to function socially (maintaining a social network, thriving at
work, etc.). It is therefore crucial to understand how language
abilities develop in children with ASD, and which factors affect them
(to figure out e.g. how a child will develop in the future and whether
there is a need for language therapy). However, language impairment is
always quantified by relying on the parent, teacher or clinician
subjective judgment of the child, and measured very sparcely (e.g. at 3
years of age and again at 6).

In this study we videotaped circa 30 kids with ASD and circa 30
comparison kids (matched by linguistic performance at visit 1) for ca.
30 minutes of naturalistic interactions with a parent. We repeated the
data collection 6 times per kid, with 4 months between each visit. We
transcribed the data and counted: i) the amount of words that each kid
uses in each video. Same for the parent. ii) the amount of unique words
that each kid uses in each video. Same for the parent. iii) the amount
of morphemes per utterance (Mean Length of Utterance) displayed by each
child in each video. Same for the parent.

Different researchers involved in the project provide you with different
datasets: 1) demographic and df data about the children (recorded by a
df psychologist) 2) length of utterance data (calculated by a linguist)
3) amount of unique and total words used (calculated by a fumbling
jack-of-all-trade, let’s call him RF)

Your job in this assignment is to double check the data and make sure
that it is ready for the analysis proper (Assignment 2), in which we
will try to understand how the children’s language develops as they grow
as a function of cognitive and social factors and which are the “cues”
suggesting a likely future language impairment.

1. Let’s get started on GitHub
------------------------------

In the assignments you will be asked to upload your code on Github and
the GitHub repositories will be part of the portfolio, therefore all
students must make an account and link it to their RStudio (you’ll thank
us later for this!).

Follow the link to one of the tutorials indicated in the syllabus: \*
Recommended:
<a href="https://happygitwithr.com/" class="uri">https://happygitwithr.com/</a>
\* Alternative (if the previous doesn’t work):
<a href="https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN" class="uri">https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN</a>
\* Alternative (if the previous doesn’t work):
<a href="https://docs.google.com/document/d/1WvApy4ayQcZaLRpD6bvAqhWncUaPmmRimT016-PrLBk/mobilebasic" class="uri">https://docs.google.com/document/d/1WvApy4ayQcZaLRpD6bvAqhWncUaPmmRimT016-PrLBk/mobilebasic</a>

N.B. Create a GitHub repository for the Assignment 1 and link it to a
project on your RStudio.

2. Now let’s take dirty dirty data sets and make them into a tidy one
---------------------------------------------------------------------

If you’re not in a project in Rstudio, make sure to set your working
directory here. If you created an RStudio project, then your working
directory (the directory with your data and code for these assignments)
is the project directory.

``` r
getwd()
```

    ## [1] "/Users/rebeccakjeldsen/Dropbox/CogSci3/ExMet3/Portfolios/1/portfolio1"

``` r
pacman::p_load(tidyverse,janitor)
```

Load the three data sets, after downloading them from dropbox and saving
them in your working directory: \* Demographic data for the
participants:
<a href="https://www.dropbox.com/s/w15pou9wstgc8fe/demo_train.csv?dl=0" class="uri">https://www.dropbox.com/s/w15pou9wstgc8fe/demo_train.csv?dl=0</a>
\* Length of utterance data:
<a href="https://www.dropbox.com/s/usyauqm37a76of6/LU_train.csv?dl=0" class="uri">https://www.dropbox.com/s/usyauqm37a76of6/LU_train.csv?dl=0</a>
\* Word data:
<a href="https://www.dropbox.com/s/8ng1civpl2aux58/token_train.csv?dl=0" class="uri">https://www.dropbox.com/s/8ng1civpl2aux58/token_train.csv?dl=0</a>

``` r
#Load datasets
demo <- read.csv("demo_train.csv")
LU <- read.csv("LU_train.csv")
word <- read.csv("token_train.csv")
```

Explore the 3 datasets (e.g. visualize them, summarize them, etc.). You
will see that the data is messy, since the psychologist collected the
demographic data, the linguist analyzed the length of utterance in May
2014 and the fumbling jack-of-all-trades analyzed the words several
months later. In particular: - the same variables might have different
names (e.g. participant and visit identifiers) - the same variables
might report the values in different ways (e.g. participant and visit
IDs) Welcome to real world of messy data :-)

Before being able to combine the data sets we need to make sure the
relevant variables have the same names and the same kind of values.

So:

2a. Identify which variable names do not match (that is are spelled
differently) and find a way to transform variable names. Pay particular
attention to the variables indicating participant and visit.

Tip: look through the chapter on data transformation in R for data
science
(<a href="http://r4ds.had.co.nz" class="uri">http://r4ds.had.co.nz</a>).
Alternatively you can look into the package dplyr (part of tidyverse),
or google “how to rename variables in R”. Or check the janitor R
package. There are always multiple ways of solving any problem and no
absolute best method.

``` r
#Look at data
names(demo)
```

    ##  [1] "Child.ID"                   "Visit"                     
    ##  [3] "Ethnicity"                  "Diagnosis"                 
    ##  [5] "ASD_check"                  "ASD2"                      
    ##  [7] "Gender"                     "Birthdate"                 
    ##  [9] "Age"                        "Total..Understands...Says."
    ## [11] "Total..Understands."        "Total.of.Both"             
    ## [13] "Age2"                       "ADOS"                      
    ## [15] "CARS"                       "CDI1"                      
    ## [17] "VinelandStandardScore"      "VinelandReceptive"         
    ## [19] "VinelandExpressive"         "VinelandWritten"           
    ## [21] "DailyLivingSkills"          "Socialization"             
    ## [23] "MotorSkills"                "MullenRaw"                 
    ## [25] "MullenTScore"               "MullenAge"                 
    ## [27] "FineMotorRaw"               "FineMotorTScore"           
    ## [29] "FIneMotorAge"               "ReceptiveLanguageRaw"      
    ## [31] "ReceptiveLanguageTScore"    "ReceptiveLanguageAge"      
    ## [33] "ExpressiveLangRaw"          "ExpressiveLangTScore"      
    ## [35] "ExpressiveLangAge"          "EarlyLearningComposite"

``` r
names(LU)
```

    ##  [1] "SUBJ"      "VISIT"     "MOT_MLU"   "MOT_LUstd" "MOT_LU_q1"
    ##  [6] "MOT_LU_q2" "MOT_LU_q3" "CHI_MLU"   "CHI_LUstd" "CHI_LU_q1"
    ## [11] "CHI_LU_q2" "CHI_LU_q3"

``` r
names(word)
```

    ## [1] "SUBJ"         "VISIT"        "types_MOT"    "types_CHI"   
    ## [5] "types_shared" "tokens_MOT"   "tokens_CHI"   "X"

``` r
head(demo)
```

    ##   Child.ID Visit Ethnicity Diagnosis ASD_check ASD2 Gender Birthdate   Age
    ## 1     A.A.     1     White         B         0    0      1  18/02/09 18.07
    ## 2     A.D.     1     White         B         0    0      1  20/12/04 19.80
    ## 3     A.D.     2     White         B         0    0      1  20/12/04 23.93
    ## 4     A.D.     3     White         B         0    0      1  20/12/04 27.70
    ## 5     A.D.     4     White         B         0    0      1  20/12/04 32.90
    ## 6     A.D.     5     White         B         0    0      1  20/12/04 35.90
    ##   Total..Understands...Says. Total..Understands. Total.of.Both  Age2 ADOS
    ## 1                          0                 183           183 18.07   15
    ## 2                         16                 245           261 19.80    0
    ## 3                         NA                  NA           133 23.93   NA
    ## 4                         NA                  NA           563 27.70   NA
    ## 5                         NA                  NA            76 32.53   NA
    ## 6                         NA                  NA            95 35.90    0
    ##   CARS CDI1 VinelandStandardScore VinelandReceptive VinelandExpressive
    ## 1   29  183                    90                20                 19
    ## 2   16  261                   100                26                 19
    ## 3   NA   NA                   100                27                 31
    ## 4   NA   NA                   103                26                 57
    ## 5   NA   NA                   107                29                 73
    ## 6   15   NA                   110                29                 80
    ##   VinelandWritten DailyLivingSkills Socialization MotorSkills MullenRaw
    ## 1              NA               121           104         111        NA
    ## 2              NA               119           108         111        28
    ## 3              NA               113           110         113        NA
    ## 4              NA               109           109         114        NA
    ## 5              NA               102           102         105        33
    ## 6              NA               107           107         114        NA
    ##   MullenTScore MullenAge FineMotorRaw FineMotorTScore FIneMotorAge
    ## 1           NA        NA           NA              NA           NA
    ## 2           66        26           22              52           21
    ## 3           NA        NA           NA              NA           NA
    ## 4           NA        NA           NA              NA           NA
    ## 5           53        33           NA              NA           NA
    ## 6           NA        NA           NA              NA           NA
    ##   ReceptiveLanguageRaw ReceptiveLanguageTScore ReceptiveLanguageAge
    ## 1                   NA                      NA                   NA
    ## 2                   28                      72                   30
    ## 3                   NA                      NA                   NA
    ## 4                   NA                      NA                   NA
    ## 5                   NA                      NA                   NA
    ## 6                   NA                      NA                   NA
    ##   ExpressiveLangRaw ExpressiveLangTScore ExpressiveLangAge
    ## 1                NA                   NA                NA
    ## 2                14                   33                14
    ## 3                NA                   NA                NA
    ## 4                NA                   NA                NA
    ## 5                NA                   NA                NA
    ## 6                NA                   NA                NA
    ##   EarlyLearningComposite
    ## 1                     NA
    ## 2                    112
    ## 3                     NA
    ## 4                     NA
    ## 5                     NA
    ## 6                     NA

``` r
#Transform variable names 
demo <- plyr::rename(demo, c("Child.ID" = "SUBJ" , "Visit"="VISIT"))
```

2b. Find a way to homogeneize the way “visit” is reported (visit1
vs. 1).

Tip: The stringr package is what you need. str\_extract () will allow
you to extract only the digit (number) from a string, by using the
regular expression \\d.

``` r
#Remove letters "visit1." -> "1."
LU$VISIT <- str_extract(LU$VISIT, "\\d")
word$VISIT <- str_extract(word$VISIT, "\\d")
```

2c. We also need to make a small adjustment to the content of the
Child.ID coloumn in the demographic data. Within this column, names that
are not abbreviations do not end with “.” (i.e. Adam), which is the case
in the other two data sets (i.e. Adam.). If The content of the two
variables isn’t identical the rows will not be merged. A neat way to
solve the problem is simply to remove all “.” in all datasets.

Tip: stringr is helpful again. Look up str\_replace\_all Tip: You can
either have one line of code for each child name that is to be changed
(easier, more typing) or specify the pattern that you want to match
(more complicated: look up “regular expressions”, but less typing)

``` r
#Remove full stops in SUBJ
demo$SUBJ <- str_replace_all(demo$SUBJ, "[[:punct:]]", "")
LU$SUBJ <- str_replace_all(LU$SUBJ, "[[:punct:]]", "")
word$SUBJ <- str_replace_all(word$SUBJ, "[[:punct:]]", "")

# A character class is a list of characters enclosed between [ and ] which matches any single character in that list

# make all VISIT as numeric
demo$VISIT <- as.numeric(demo$VISIT)
LU$VISIT <- as.numeric(LU$VISIT)
word$VISIT <- as.numeric(word$VISIT)
```

2d. Now that the nitty gritty details of the different data sets are
fixed, we want to make a subset of each data set only containig the
variables that we wish to use in the final data set. For this we use the
tidyverse package dplyr, which contains the function select().

The variables we need are: \* Child.ID, √ \* Visit, √ \* Diagnosis, √ \*
Ethnicity, √ \* Gender, √ \* Age, √ \* ADOS, √ \* MullenRaw √ \*
ExpressiveLangRaw, √ \* Socialization √ \* MOT\_MLU, √ \* CHI\_MLU, √ \*
types\_MOT, \* types\_CHI, \* tokens\_MOT, \* tokens\_CHI. In total: 16

Most variables should make sense, here the less intuitive ones. \* ADOS
(Autism Diagnostic Observation Schedule) indicates the severity of the
autistic symptoms (the higher the score, the worse the symptoms). Ref:
<a href="https://link.springer.com/article/10.1023/A:1005592401947" class="uri">https://link.springer.com/article/10.1023/A:1005592401947</a>
\* MLU stands for mean length of utterance (usually a proxy for
syntactic complexity) \* types stands for unique words (e.g. even if
“doggie” is used 100 times it only counts for 1) \* tokens stands for
overall amount of words (if “doggie” is used 100 times it counts for
100) \* MullenRaw indicates non verbal IQ, as measured by Mullen Scales
of Early Learning (MSEL
<a href="https://link.springer.com/referenceworkentry/10.1007%2F978-1-4419-1698-3_596" class="uri">https://link.springer.com/referenceworkentry/10.1007%2F978-1-4419-1698-3_596</a>)
\* ExpressiveLangRaw indicates verbal IQ, as measured by MSEL \*
Socialization indicates social interaction skills and social
responsiveness, as measured by Vineland
(<a href="https://cloudfront.ualberta.ca/-/media/ualberta/faculties-and-programs/centres-institutes/community-university-partnership/resources/tools---assessment/vinelandjune-2012.pdf" class="uri">https://cloudfront.ualberta.ca/-/media/ualberta/faculties-and-programs/centres-institutes/community-university-partnership/resources/tools---assessment/vinelandjune-2012.pdf</a>)

Feel free to rename the variables into something you can remember
(i.e. nonVerbalIQ, verbalIQ)

``` r
#Rename 
demo <- plyr::rename(demo, c("MullenRaw" = "nonVerbalIQ", "ExpressiveLangRaw" = "verbalIQ", "ADOS" = "severity"))

word <- plyr::rename(word, c("types_MOT" = "uniqueW_MOT", "types_CHI" = "uniqueW_CHI", "tokens_MOT" = "totalW_MOT", "tokens_CHI" = "totalW_CHI"))

#Making subsets
demoSub <- subset(demo, select = c(SUBJ:Diagnosis, Gender, Age, severity, Socialization, nonVerbalIQ, verbalIQ))
LUSub <- subset(LU, select= c(SUBJ, VISIT, MOT_MLU, CHI_MLU))
wordSub <- subset(word, select= c(SUBJ, VISIT, uniqueW_MOT, uniqueW_CHI, totalW_MOT, totalW_CHI))
```

2e. Finally we are ready to merge all the data sets into just one.

Some things to pay attention to: \* make sure to check that the merge
has included all relevant data (e.g. by comparing the number of rows) \*
make sure to understand whether (and if so why) there are NAs in the
dataset (e.g. some measures were not taken at all visits, some
recordings were lost or permission to use was withdrawn)

``` r
#Merge LU and word subsets
dfMerge <- merge(LUSub, wordSub, by = c("SUBJ", "VISIT"), all=T)

#Final merge
df <- merge(dfMerge, demoSub, by = c("SUBJ", "VISIT"), all=T)

summary(is.na(df))
```

    ##     SUBJ           VISIT          MOT_MLU         CHI_MLU       
    ##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
    ##  FALSE:372       FALSE:372       FALSE:352       FALSE:352      
    ##                                  TRUE :20        TRUE :20       
    ##  uniqueW_MOT     uniqueW_CHI     totalW_MOT      totalW_CHI     
    ##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
    ##  FALSE:352       FALSE:352       FALSE:352       FALSE:352      
    ##  TRUE :20        TRUE :20        TRUE :20        TRUE :20       
    ##  Ethnicity       Diagnosis         Gender           Age         
    ##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
    ##  FALSE:372       FALSE:372       FALSE:372       FALSE:362      
    ##                                                  TRUE :10       
    ##   severity       Socialization   nonVerbalIQ      verbalIQ      
    ##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
    ##  FALSE:125       FALSE:369       FALSE:182       FALSE:123      
    ##  TRUE :247       TRUE :3         TRUE :190       TRUE :249

2f. Only using df measures from Visit 1 In order for our models to be
useful, we want to miimize the need to actually test children as they
develop. In other words, we would like to be able to understand and
predict the children’s linguistic development after only having tested
them once. Therefore we need to make sure that our ADOS (severity),
MullenRaw(nonverbalIQ), ExpressiveLangRaw(verbalIQ) and Socialization
variables are reporting (for all visits) only the scores from visit 1.

A possible way to do so: \* create a new dataset with only visit 1,
child id and the 4 relevant df variables to be merged with the old
dataset \* rename the df variables (e.g. ADOS to ADOS1) and remove the
visit (so that the new df variables are reported for all 6 visits) \*
merge the new dataset with the old

``` r
#Using first possible way with subset() - new df w/ only visit 1, child id and the 4 relevant df variables
subdf <- subset(df, VISIT == 1, select = c(SUBJ, severity, Socialization, nonVerbalIQ, verbalIQ))

#rename ADOS 
subdf <- plyr::rename(subdf, c("severity" = "severity1", "Socialization" = "Socialization1" , "nonVerbalIQ" = "nonVerbalIQ1", "verbalIQ" = "verbalIQ1"))


#Merge w/ old dataset 
df <- merge(subdf, df, by = "SUBJ")
```

2g. Final touches

Now we want to \* anonymize our participants (they are real children!).
\* make sure the variables have sensible values. E.g. right now gender
is marked 1 and 2, but in two weeks you will not be able to remember,
which gender were connected to which number, so change the values from 1
and 2 to F and M in the gender variable. For the same reason, you should
also change the values of Diagnosis from A and B to ASD (autism spectrum
disorder) and TD (typically developing). Tip: Try taking a look at
ifelse(), or google “how to rename levels in R”. \* Save the data set
using into a csv file. Hint: look into write.csv()

``` r
# anonymize our participants (they are real children!). 
df$SUBJ <- as.numeric(as.factor(df$SUBJ))

#Gender from number to letters
df$Gender <- ifelse(df$Gender == "1","M","F")

#from factor to character
df$Diagnosis <- as.character(df$Diagnosis)

#Diagnosis from single letter to name
df$Diagnosis[df$Diagnosis == "A"] <- "ASD"
df$Diagnosis[df$Diagnosis == "B"] <- "TD"

#Ohter adjustments for a clean template 
df$Ethnicity[df$Ethnicity=="Bangledeshi"] <- "Bangladeshi"

write.csv(df, "clean.csv")
```

1.  BONUS QUESTIONS The aim of this last section is to make sure you are
    fully fluent in the tidyverse. Here’s the link to a very helpful
    book, which explains each function:
    <a href="http://r4ds.had.co.nz/index.html" class="uri">http://r4ds.had.co.nz/index.html</a>

2.  USING FILTER List all kids who:

<!-- -->

1.  have a mean length of utterance (across all visits) of more than 2.7
    morphemes.
2.  have a mean length of utterance of less than 1.5 morphemes at the
    first visit
3.  have not completed all trials. Tip: Use pipes to solve this

``` r
# 1
df %>% 
  group_by(SUBJ) %>% 
  summarise(mMLU = mean(CHI_MLU, na.rm = T)) %>% 
  filter(mMLU > 2.7)
```

    ## # A tibble: 11 x 2
    ##     SUBJ  mMLU
    ##    <dbl> <dbl>
    ##  1     2  3.31
    ##  2     3  3.02
    ##  3     4  3.13
    ##  4    13  2.75
    ##  5    19  2.84
    ##  6    27  2.73
    ##  7    28  3.50
    ##  8    44  2.75
    ##  9    53  2.94
    ## 10    57  3.16
    ## 11    64  3.26

``` r
# 2
df %>% 
  filter(VISIT == 1 & CHI_MLU < 1.5)
```

    ##    SUBJ severity1 Socialization1 nonVerbalIQ1 verbalIQ1 VISIT  MOT_MLU
    ## 1     1         0            108           28        14     1 3.621993
    ## 2     5         9             82           34        27     1 3.986357
    ## 3     6        17             68           20        17     1 2.618729
    ## 4     7        18             65           24        14     1 2.244755
    ## 5     9         0            100           24        18     1 3.544419
    ## 6    10         3            104           27        18     1 4.204846
    ## 7    12         0            106           21        15     1 3.380463
    ## 8    13         0            104           30        16     1 4.195335
    ## 9    15         0             92           23        17     1 3.420315
    ## 10   16         0             86           24        15     1 3.967078
    ## 11   18        14             74           25        11     1 3.182390
    ## 12   20        11             88           28        20     1 2.539823
    ## 13   21         9             86           26        14     1 2.524740
    ## 14   22        21             72           22         8     1 4.390879
    ## 15   24         0            106           21        19     1 3.630476
    ## 16   25        14             65           25        19     1 3.024548
    ## 17   26        20             67           13        11     1 2.917355
    ## 18   29         0             96           26        18     1 3.616867
    ## 19   30         0            102           20        16     1 2.788793
    ## 20   31        17             72           26        14     1 3.304189
    ## 21   32        12             70           31        13     1 3.607088
    ## 22   35        14             76           25        11     1 2.287293
    ## 23   36        10             82           27        22     1 2.743455
    ## 24   37         1            102           23        21     1 3.561364
    ## 25   38         3             98           19        13     1 3.921109
    ## 26   39         7             70           33        26     1 4.135036
    ## 27   40         1            104           29        28     1 3.420975
    ## 28   41        11             88           26        19     1 3.298748
    ## 29   42        13             86           27        13     1 2.997050
    ## 30   43         0            102           25        17     1 3.093146
    ## 31   44         0             96           24        19     1 4.033333
    ## 32   46         3             94           30        20     1 3.088757
    ## 33   47         5            113           24        20     1 2.776181
    ## 34   49        20             75           21         9     1 4.883966
    ## 35   51         0            102           27        20     1 3.943005
    ## 36   52        17             82           28        10     1 3.765528
    ## 37   53         0            108           27        27     1 4.030075
    ## 38   54        19             64           17        10     1 3.704110
    ## 39   55         1            102           22        14     1 3.435743
    ## 40   56         0             94           29        22     1 5.344227
    ## 41   58         1            115           24        22     1 3.487871
    ## 42   59         0             96           26        17     1 3.509138
    ## 43   60        14             77           27        11     1 2.548969
    ## 44   61        15             66           28        10     1 3.833770
    ## 45   62        15             88           30        24     1 2.747100
    ## 46   63         4            108           29        22     1 3.432387
    ## 47   65        15             79           27        16     1 3.030405
    ##      CHI_MLU uniqueW_MOT uniqueW_CHI totalW_MOT totalW_CHI
    ## 1  1.2522523         378          14       1835        139
    ## 2  1.3947368         324          57       2859        197
    ## 3  1.0000000         212           4        761         29
    ## 4  1.2641509         152          29        578        130
    ## 5  1.0378788         363          36       1408        137
    ## 6  1.0375000         289          15       1808         83
    ## 7  1.2168675         215          24       1136        101
    ## 8  1.0877193         235          17       1262         62
    ## 9  1.0396040         287           7       1625        105
    ## 10 1.1647059         277          27       1643         99
    ## 11 1.0277778         281           9       1418         37
    ## 12 1.3595506         283          89       1019        227
    ## 13 0.1857143         321          16       1787        214
    ## 14 1.0000000         485           8       2826        122
    ## 15 1.2371134         343          36       1698        118
    ## 16 1.4324324         328          41       2138        103
    ## 17 1.0833333         193           6        654         26
    ## 18 1.3661972         317          37       1361         95
    ## 19 1.2758621         178          34        584        111
    ## 20 1.0086207         295           6       1643        117
    ## 21 0.9000000         366          11       2054         21
    ## 22 1.2500000         206          13        788         35
    ## 23 1.3766234         214          67        893        180
    ## 24 1.2641509         291          24       1344        260
    ## 25 1.2307692         281           8       1631         16
    ## 26 0.4805825         381          39       1988        337
    ## 27 1.3322785         342          96       2035        398
    ## 28 1.2043011         274          36       1537        109
    ## 29 1.0175439         252           9       1827         58
    ## 30 1.0262009         333          32       1547        235
    ## 31 1.3034483         373          47       2334        176
    ## 32 1.2592593         275           9       1417         68
    ## 33 0.5584416         258          20       1188         91
    ## 34 1.1666667         387          10       2144         63
    ## 35 1.0761421         260          13       1347        212
    ## 36 1.2500000         303          17       2147         40
    ## 37 1.4258373         255          73       1452        286
    ## 38 1.1000000         265           8       1215         43
    ## 39 1.1818182         331           7       1503         38
    ## 40 1.4086957         441          92       2267        319
    ## 41 1.3139535         214          32       1118        109
    ## 42 1.1846154         178          11       1144        154
    ## 43 1.0444444         195           9        955         47
    ## 44 0.0000000         386           0       2613          0
    ## 45 1.1809045         338          98       2084        233
    ## 46 1.1830065         345          62       1805        244
    ## 47 1.0375000         303          15       1579        166
    ##           Ethnicity Diagnosis Gender   Age severity Socialization
    ## 1             White        TD      M 19.80        0           108
    ## 2             White       ASD      M 34.03        9            82
    ## 3       Bangladeshi       ASD      F 26.17       17            68
    ## 4             White       ASD      F 41.00       18            65
    ## 5             White        TD      F 18.30        0           100
    ## 6             White        TD      M 19.27        3           104
    ## 7             White        TD      M 19.23        0           106
    ## 8             White        TD      M 20.07        0           104
    ## 9             White        TD      M 18.97        0            92
    ## 10            White        TD      M 19.27        0            86
    ## 11            White       ASD      M 34.80       14            74
    ## 12            White       ASD      M 35.80       11            88
    ## 13            White       ASD      M 18.77        9            86
    ## 14 African American       ASD      M 27.53       21            72
    ## 15            White        TD      F 18.93        0           106
    ## 16     White/Latino       ASD      M 27.37       14            65
    ## 17            White       ASD      M 37.47       20            67
    ## 18            White        TD      M 21.03        0            96
    ## 19            White        TD      M 19.93        0           102
    ## 20            White       ASD      M 34.87       17            72
    ## 21            White       ASD      M 36.53       12            70
    ## 22 African American       ASD      F 25.33       14            76
    ## 23            White       ASD      M 33.77       10            82
    ## 24            White        TD      M 19.30        1           102
    ## 25            White        TD      M 19.20        3            98
    ## 26            White       ASD      M 39.50        7            70
    ## 27            White        TD      F 19.87        1           104
    ## 28      White/Asian       ASD      M 33.20       11            88
    ## 29         Lebanese       ASD      M 24.90       13            86
    ## 30            White        TD      M 19.23        0           102
    ## 31            White        TD      M 19.37        0            96
    ## 32            White        TD      M 19.77        3            94
    ## 33            White        TD      M 20.03        5           113
    ## 34            White       ASD      M 36.73       20            75
    ## 35            White        TD      F 20.03        0           102
    ## 36            White       ASD      M 31.63       17            82
    ## 37            White        TD      M 23.07        0           108
    ## 38            White       ASD      M 37.47       19            64
    ## 39            Asian        TD      F 20.87        1           102
    ## 40            White        TD      M 22.57        0            94
    ## 41            White        TD      M 19.10        1           115
    ## 42            White        TD      M 19.97        0            96
    ## 43            White       ASD      M 35.50       14            77
    ## 44            White       ASD      F 41.07       15            66
    ## 45            White       ASD      M 26.00       15            88
    ## 46            White        TD      M 20.80        4           108
    ## 47            White       ASD      M 42.00       15            79
    ##    nonVerbalIQ verbalIQ
    ## 1           28       14
    ## 2           34       27
    ## 3           20       17
    ## 4           24       14
    ## 5           24       18
    ## 6           27       18
    ## 7           21       15
    ## 8           30       16
    ## 9           23       17
    ## 10          24       15
    ## 11          25       11
    ## 12          28       20
    ## 13          26       14
    ## 14          22        8
    ## 15          21       19
    ## 16          25       19
    ## 17          13       11
    ## 18          26       18
    ## 19          20       16
    ## 20          26       14
    ## 21          31       13
    ## 22          25       11
    ## 23          27       22
    ## 24          23       21
    ## 25          19       13
    ## 26          33       26
    ## 27          29       28
    ## 28          26       19
    ## 29          27       13
    ## 30          25       17
    ## 31          24       19
    ## 32          30       20
    ## 33          24       20
    ## 34          21        9
    ## 35          27       20
    ## 36          28       10
    ## 37          27       27
    ## 38          17       10
    ## 39          22       14
    ## 40          29       22
    ## 41          24       22
    ## 42          26       17
    ## 43          27       11
    ## 44          28       10
    ## 45          30       24
    ## 46          29       22
    ## 47          27       16

``` r
# 3
df %>% 
  group_by(SUBJ) %>% 
  summarise(na= sum(is.na(CHI_MLU)), visit = n()) %>% 
  filter(na > 0 | visit < 6)
```

    ## # A tibble: 18 x 3
    ##     SUBJ    na visit
    ##    <dbl> <int> <int>
    ##  1     2     1     6
    ##  2     7     1     6
    ##  3     8     1     6
    ##  4     9     1     6
    ##  5    11     2     2
    ##  6    18     1     6
    ##  7    23     3     3
    ##  8    28     1     6
    ##  9    40     1     6
    ## 10    42     1     6
    ## 11    46     1     6
    ## 12    47     0     5
    ## 13    48     2     2
    ## 14    50     2     2
    ## 15    52     0     5
    ## 16    59     1     6
    ## 17    60     0     4
    ## 18    66     1     1

USING ARRANGE

1.  Sort kids to find the kid who produced the most words on the 6th
    visit
2.  Sort kids to find the kid who produced the least amount of words on
    the 1st visit.

``` r
# 1 = subj
df %>% 
  filter(VISIT == 6) %>% 
  arrange(desc(totalW_CHI))
```

    ##    SUBJ severity1 Socialization1 nonVerbalIQ1 verbalIQ1 VISIT  MOT_MLU
    ## 1    59         0             96           26        17     6 3.957230
    ## 2    28        11            100           32        33     6 4.111413
    ## 3    19         0            106           29        33     6 4.013353
    ## 4    27         0             90           29        22     6 4.472993
    ## 5    45        14             65           42        27     6 4.250853
    ## 6    39         7             70           33        26     6 4.240798
    ## 7    14         0            102           25        17     6 4.847418
    ## 8    24         0            106           21        19     6 4.211321
    ## 9    15         0             92           23        17     6 4.664286
    ## 10    4         8             82           31        27     6 3.532374
    ## 11   44         0             96           24        19     6 3.391525
    ## 12   64        13             87           30        30     6 4.080446
    ## 13   17         0            102           29        26     6 4.445872
    ## 14   43         0            102           25        17     6 4.061475
    ## 15    2        13             85           34        27     6 4.588477
    ## 16    1         0            108           28        14     6 4.664013
    ## 17   12         0            106           21        15     6 4.287582
    ## 18   63         4            108           29        22     6 4.113158
    ## 19   53         0            108           27        27     6 5.247093
    ## 20   13         0            104           30        16     6 4.468493
    ## 21   62        15             88           30        24     6 3.320000
    ## 22   37         1            102           23        21     6 4.239198
    ## 23   36        10             82           27        22     6 3.370937
    ## 24   21         9             86           26        14     6 5.379798
    ## 25   42        13             86           27        13     6 3.926928
    ## 26   38         3             98           19        13     6 4.388235
    ## 27    5         9             82           34        27     6 4.587179
    ## 28   29         0             96           26        18     6 4.254505
    ## 29   30         0            102           20        16     6 4.239437
    ## 30   58         1            115           24        22     6 4.186161
    ## 31    3         1             88           29        18     6 5.229885
    ## 32   56         0             94           29        22     6 5.587332
    ## 33   33         0            100           27        22     6 3.603473
    ## 34   16         0             86           24        15     6 4.347709
    ## 35   55         1            102           22        14     6 5.153639
    ## 36   57         0             98           30        30     6 3.706790
    ## 37   52        17             82           28        10     6 3.370093
    ## 38    6        17             68           20        17     6 2.483146
    ## 39   20        11             88           28        20     6 3.156695
    ## 40   25        14             65           25        19     6 3.534173
    ## 41   65        15             79           27        16     6 3.514403
    ## 42   31        17             72           26        14     6 4.349353
    ## 43    8         5            102           32        31     6 4.366972
    ## 44   32        12             70           31        13     6 4.341549
    ## 45   10         3            104           27        18     6 4.235585
    ## 46   26        20             67           13        11     6 3.860795
    ## 47   49        20             75           21         9     6 3.460548
    ## 48   41        11             88           26        19     6 4.100707
    ## 49   34        21             69           21         9     6 4.003257
    ## 50   35        14             76           25        11     6 3.965392
    ## 51   61        15             66           28        10     6 3.241422
    ## 52   51         0            102           27        20     6 4.676101
    ## 53   18        14             74           25        11     6 3.650235
    ## 54   60        14             77           27        11     6 3.474725
    ## 55   54        19             64           17        10     6 3.886842
    ## 56   22        21             72           22         8     6 3.943636
    ## 57    7        18             65           24        14     6       NA
    ## 58    9         0            100           24        18     6       NA
    ## 59   40         1            104           29        28     6       NA
    ## 60   46         3             94           30        20     6       NA
    ##      CHI_MLU uniqueW_MOT uniqueW_CHI totalW_MOT totalW_CHI
    ## 1  2.9092742         367         260       1731       1294
    ## 2  3.3643411         452         273       3076       1249
    ## 3  2.9095355         516         235       2576       1079
    ## 4  3.0614525         555         237       2895       1010
    ## 5  2.6794872         374         219       2227        921
    ## 6  3.0774194         429         217       2510        897
    ## 7  3.7011952         388         210       1959        864
    ## 8  3.0911950         478         221       2044        847
    ## 9  3.8114035         359         178       1802        793
    ## 10 3.2781955         410         166       2171        738
    ## 11 3.0727969         357         219       1764        719
    ## 12 3.4415584         505         226       3072        713
    ## 13 2.9482072         491         250       2264        710
    ## 14 2.8526316         397         213       1741        702
    ## 15 3.4135021         304         245        999        698
    ## 16 2.8651685         595         210       2586        686
    ## 17 2.7571429         260         168       1138        666
    ## 18 2.8480000         303         158       1460        659
    ## 19 3.5950000         383         217       1701        646
    ## 20 3.5049505         339         201       1498        640
    ## 21 2.1553398         335         197       2221        618
    ## 22 2.3815789         411         156       2438        611
    ## 23 2.2745098         342         157       1540        605
    ## 24 2.9027778         433         155       2389        590
    ## 25 2.1586207         417         170       2508        571
    ## 26 2.5614754         324         165       1978        568
    ## 27 2.7665198         462         179       3182        538
    ## 28 2.4800000         385         154       1788        530
    ## 29 2.9607843         391         164       1889        521
    ## 30 2.8921569         437         183       2347        517
    ## 31 3.7103448         486         173       2564        460
    ## 32 2.4292929         548         163       2887        436
    ## 33 2.0717489         475         185       2363        418
    ## 34 2.8690476         327         156       1395        410
    ## 35 2.7612903         383         140       1866        395
    ## 36 3.2439024         249         102       1024        358
    ## 37 1.4734513         319          98       1565        306
    ## 38 1.4967320         158          66        536        300
    ## 39 2.1450382         256          55        995        274
    ## 40 1.3052632         372          47       1748        236
    ## 41 1.4012346         311          58       1481        210
    ## 42 1.2883436         454          52       2391        204
    ## 43 2.2258065         322          64       1352        197
    ## 44 1.0843373         425         101       2219        195
    ## 45 2.7051282         400          73       2271        189
    ## 46 1.1680000         309          62       1349        143
    ## 47 1.1610169         351          10       1918        137
    ## 48 1.6447368         330          55       1987        110
    ## 49 2.6315789         585          12       2202        100
    ## 50 0.7536232         326          14       1852         79
    ## 51 0.0156250         444           2       2450         64
    ## 52 2.7600000         322          34       1371         61
    ## 53 1.0000000         281           3       1454         37
    ## 54 1.0588235         284           6       1390         36
    ## 55 1.3333333         370           4       1396          8
    ## 56 0.5000000         388           2       2077          2
    ## 57        NA          NA          NA         NA         NA
    ## 58        NA          NA          NA         NA         NA
    ## 59        NA          NA          NA         NA         NA
    ## 60        NA          NA          NA         NA         NA
    ##           Ethnicity Diagnosis Gender   Age severity Socialization
    ## 1             White        TD      M 41.93       NA            95
    ## 2             White       ASD      M 51.00       NA            90
    ## 3             White        TD      M 44.07       NA           116
    ## 4             White        TD      M 42.47       NA           103
    ## 5             White       ASD      M 57.37       NA            77
    ## 6             White       ASD      M 58.77       NA           116
    ## 7             White        TD      M 39.40       NA            92
    ## 8             White        TD      F 39.23       NA           110
    ## 9             White        TD      M 39.43       NA            83
    ## 10     White/Latino       ASD      M 51.37       NA            74
    ## 11            White        TD      M 40.13       NA           101
    ## 12            White       ASD      M 54.73       NA            97
    ## 13            White        TD      M 42.93       NA            97
    ## 14            White        TD      M 39.93       NA           101
    ## 15            White       ASD      M 49.70       NA            81
    ## 16            White        TD      M 40.13       NA           100
    ## 17            White        TD      M 40.27       NA           101
    ## 18            White        TD      M 41.00       NA            NA
    ## 19            White        TD      M 43.40       NA           101
    ## 20            White        TD      M 41.50       NA            97
    ## 21            White       ASD      M 46.07       NA            79
    ## 22            White        TD      M 39.07       NA            97
    ## 23            White       ASD      M 53.77       NA            92
    ## 24            White       ASD      M 37.30       NA           103
    ## 25         Lebanese       ASD      M 46.40       NA            94
    ## 26            White        TD      M 38.53       NA            97
    ## 27            White       ASD      M 54.13       NA            75
    ## 28            White        TD      M 41.17       NA           106
    ## 29            White        TD      M 39.43       NA            95
    ## 30            White        TD      M 39.30       NA           101
    ## 31            White        TD      F 45.07       NA            86
    ## 32            White        TD      M 43.03       NA            97
    ## 33            White        TD      M 43.80       NA           108
    ## 34            White        TD      M 40.30       NA            94
    ## 35            Asian        TD      F 42.10       NA           108
    ## 36            White        TD      M 44.43       NA           101
    ## 37            White       ASD      M    NA       NA            63
    ## 38      Bangladeshi       ASD      F 46.53       NA            74
    ## 39            White       ASD      M 56.73       NA            72
    ## 40     White/Latino       ASD      M 47.50       NA            65
    ## 41            White       ASD      M 62.33       NA           103
    ## 42            White       ASD      M 55.17       NA            72
    ## 43            White        TD      M 40.17       NA           108
    ## 44            White       ASD      M 56.43       NA            85
    ## 45            White        TD      M 40.43       NA           103
    ## 46            White       ASD      M 62.40       NA            68
    ## 47            White       ASD      M 57.43       NA            59
    ## 48      White/Asian       ASD      M 53.63       NA            63
    ## 49            White       ASD      M 54.63       NA            66
    ## 50 African American       ASD      F 46.17       NA            83
    ## 51            White       ASD      F 61.70       NA            68
    ## 52            White        TD      F 40.37       NA           101
    ## 53            White       ASD      M 54.43       NA            65
    ## 54            White       ASD      M    NA       NA            NA
    ## 55            White       ASD      M 62.40       NA            66
    ## 56 African American       ASD      M 48.97       NA            65
    ## 57            White       ASD      F 60.33       NA            59
    ## 58            White        TD      F    NA       NA            NA
    ## 59            White        TD      F 40.23       NA           114
    ## 60            White        TD      M 39.93       NA           118
    ##    nonVerbalIQ verbalIQ
    ## 1           45       38
    ## 2           46       46
    ## 3           50       50
    ## 4           48       39
    ## 5           49       45
    ## 6           50       48
    ## 7           47       41
    ## 8           43       40
    ## 9           44       37
    ## 10          44       44
    ## 11          46       46
    ## 12          50       50
    ## 13          49       46
    ## 14          45       36
    ## 15          48       48
    ## 16          42       44
    ## 17          42       27
    ## 18          NA       NA
    ## 19          45       48
    ## 20          44       47
    ## 21          45       30
    ## 22          45       34
    ## 23          44       32
    ## 24          43       48
    ## 25          41       37
    ## 26          46       31
    ## 27          40       37
    ## 28          43       35
    ## 29          39       32
    ## 30          41       41
    ## 31          45       45
    ## 32          47       44
    ## 33          45       45
    ## 34          40       40
    ## 35          46       40
    ## 36          45       36
    ## 37          32       21
    ## 38          39       26
    ## 39          30       32
    ## 40          30       18
    ## 41          41       31
    ## 42          39       21
    ## 43          43       39
    ## 44          49       22
    ## 45          33       33
    ## 46          30       12
    ## 47          32       10
    ## 48          30       29
    ## 49          28       10
    ## 50          33       18
    ## 51          32        9
    ## 52          43       46
    ## 53          27       11
    ## 54          34       17
    ## 55          24       11
    ## 56          26       16
    ## 57          32       27
    ## 58          NA       NA
    ## 59          49       43
    ## 60          40       42

``` r
# 2 
df %>%
  filter(VISIT == 1) %>% 
  arrange(totalW_CHI)
```

    ##    SUBJ severity1 Socialization1 nonVerbalIQ1 verbalIQ1 VISIT  MOT_MLU
    ## 1    61        15             66           28        10     1 3.833770
    ## 2    34        21             69           21         9     1 3.686347
    ## 3    38         3             98           19        13     1 3.921109
    ## 4    32        12             70           31        13     1 3.607088
    ## 5    26        20             67           13        11     1 2.917355
    ## 6     6        17             68           20        17     1 2.618729
    ## 7    35        14             76           25        11     1 2.287293
    ## 8    18        14             74           25        11     1 3.182390
    ## 9    55         1            102           22        14     1 3.435743
    ## 10   52        17             82           28        10     1 3.765528
    ## 11   54        19             64           17        10     1 3.704110
    ## 12   60        14             77           27        11     1 2.548969
    ## 13   42        13             86           27        13     1 2.997050
    ## 14   13         0            104           30        16     1 4.195335
    ## 15   49        20             75           21         9     1 4.883966
    ## 16   46         3             94           30        20     1 3.088757
    ## 17   10         3            104           27        18     1 4.204846
    ## 18   47         5            113           24        20     1 2.776181
    ## 19   29         0             96           26        18     1 3.616867
    ## 20   16         0             86           24        15     1 3.967078
    ## 21   12         0            106           21        15     1 3.380463
    ## 22   25        14             65           25        19     1 3.024548
    ## 23   15         0             92           23        17     1 3.420315
    ## 24   41        11             88           26        19     1 3.298748
    ## 25   58         1            115           24        22     1 3.487871
    ## 26   30         0            102           20        16     1 2.788793
    ## 27   31        17             72           26        14     1 3.304189
    ## 28   24         0            106           21        19     1 3.630476
    ## 29   22        21             72           22         8     1 4.390879
    ## 30    7        18             65           24        14     1 2.244755
    ## 31    9         0            100           24        18     1 3.544419
    ## 32    1         0            108           28        14     1 3.621993
    ## 33    8         5            102           32        31     1 3.265082
    ## 34   59         0             96           26        17     1 3.509138
    ## 35   65        15             79           27        16     1 3.030405
    ## 36   44         0             96           24        19     1 4.033333
    ## 37   36        10             82           27        22     1 2.743455
    ## 38    5         9             82           34        27     1 3.986357
    ## 39   51         0            102           27        20     1 3.943005
    ## 40   21         9             86           26        14     1 2.524740
    ## 41   20        11             88           28        20     1 2.539823
    ## 42   62        15             88           30        24     1 2.747100
    ## 43   43         0            102           25        17     1 3.093146
    ## 44   63         4            108           29        22     1 3.432387
    ## 45   14         0            102           25        17     1 3.960254
    ## 46   17         0            102           29        26     1 4.910190
    ## 47    3         1             88           29        18     1 3.757269
    ## 48   37         1            102           23        21     1 3.561364
    ## 49    4         8             82           31        27     1 3.459370
    ## 50   53         0            108           27        27     1 4.030075
    ## 51   33         0            100           27        22     1 3.494327
    ## 52   56         0             94           29        22     1 5.344227
    ## 53   39         7             70           33        26     1 4.135036
    ## 54   40         1            104           29        28     1 3.420975
    ## 55   19         0            106           29        33     1 3.587855
    ## 56   27         0             90           29        22     1 4.643200
    ## 57   45        14             65           42        27     1 3.341418
    ## 58    2        13             85           34        27     1 4.098446
    ## 59   64        13             87           30        30     1 3.604140
    ## 60   57         0             98           30        30     1 4.159159
    ## 61   28        11            100           32        33     1 4.690751
    ## 62   11         0            105           28        33     1       NA
    ## 63   23        19             78           26        16     1       NA
    ## 64   48         0             92           27        17     1       NA
    ## 65   50         4             84           17        16     1       NA
    ## 66   66        15            104           NA        NA     1       NA
    ##      CHI_MLU uniqueW_MOT uniqueW_CHI totalW_MOT totalW_CHI
    ## 1  0.0000000         386           0       2613          0
    ## 2  1.5000000         228           3        927          3
    ## 3  1.2307692         281           8       1631         16
    ## 4  0.9000000         366          11       2054         21
    ## 5  1.0833333         193           6        654         26
    ## 6  1.0000000         212           4        761         29
    ## 7  1.2500000         206          13        788         35
    ## 8  1.0277778         281           9       1418         37
    ## 9  1.1818182         331           7       1503         38
    ## 10 1.2500000         303          17       2147         40
    ## 11 1.1000000         265           8       1215         43
    ## 12 1.0444444         195           9        955         47
    ## 13 1.0175439         252           9       1827         58
    ## 14 1.0877193         235          17       1262         62
    ## 15 1.1666667         387          10       2144         63
    ## 16 1.2592593         275           9       1417         68
    ## 17 1.0375000         289          15       1808         83
    ## 18 0.5584416         258          20       1188         91
    ## 19 1.3661972         317          37       1361         95
    ## 20 1.1647059         277          27       1643         99
    ## 21 1.2168675         215          24       1136        101
    ## 22 1.4324324         328          41       2138        103
    ## 23 1.0396040         287           7       1625        105
    ## 24 1.2043011         274          36       1537        109
    ## 25 1.3139535         214          32       1118        109
    ## 26 1.2758621         178          34        584        111
    ## 27 1.0086207         295           6       1643        117
    ## 28 1.2371134         343          36       1698        118
    ## 29 1.0000000         485           8       2826        122
    ## 30 1.2641509         152          29        578        130
    ## 31 1.0378788         363          36       1408        137
    ## 32 1.2522523         378          14       1835        139
    ## 33 1.5600000         288          59       1564        143
    ## 34 1.1846154         178          11       1144        154
    ## 35 1.0375000         303          15       1579        166
    ## 36 1.3034483         373          47       2334        176
    ## 37 1.3766234         214          67        893        180
    ## 38 1.3947368         324          57       2859        197
    ## 39 1.0761421         260          13       1347        212
    ## 40 0.1857143         321          16       1787        214
    ## 41 1.3595506         283          89       1019        227
    ## 42 1.1809045         338          98       2084        233
    ## 43 1.0262009         333          32       1547        235
    ## 44 1.1830065         345          62       1805        244
    ## 45 1.5740741         329          69       2139        249
    ## 46 1.5988372         375          90       2585        254
    ## 47 1.8776978         334          51       2674        260
    ## 48 1.2641509         291          24       1344        260
    ## 49 2.0972222         379         102       2009        269
    ## 50 1.4258373         255          73       1452        286
    ## 51 1.5333333         322          91       1870        319
    ## 52 1.4086957         441          92       2267        319
    ## 53 0.4805825         381          39       1988        337
    ## 54 1.3322785         342          96       2035        398
    ## 55 1.7948718         467         108       2555        406
    ## 56 1.5827338         373          71       2740        433
    ## 57 1.9144981         271         101       1591        450
    ## 58 2.2768595         317         146       1428        461
    ## 59 2.8763441         400         149       2587        469
    ## 60 1.9397163         236         120       1170        473
    ## 61 3.4000000         278         119       1450        483
    ## 62        NA          NA          NA         NA         NA
    ## 63        NA          NA          NA         NA         NA
    ## 64        NA          NA          NA         NA         NA
    ## 65        NA          NA          NA         NA         NA
    ## 66        NA          NA          NA         NA         NA
    ##           Ethnicity Diagnosis Gender   Age severity Socialization
    ## 1             White       ASD      F 41.07       15            66
    ## 2             White       ASD      M 34.27       21            69
    ## 3             White        TD      M 19.20        3            98
    ## 4             White       ASD      M 36.53       12            70
    ## 5             White       ASD      M 37.47       20            67
    ## 6       Bangladeshi       ASD      F 26.17       17            68
    ## 7  African American       ASD      F 25.33       14            76
    ## 8             White       ASD      M 34.80       14            74
    ## 9             Asian        TD      F 20.87        1           102
    ## 10            White       ASD      M 31.63       17            82
    ## 11            White       ASD      M 37.47       19            64
    ## 12            White       ASD      M 35.50       14            77
    ## 13         Lebanese       ASD      M 24.90       13            86
    ## 14            White        TD      M 20.07        0           104
    ## 15            White       ASD      M 36.73       20            75
    ## 16            White        TD      M 19.77        3            94
    ## 17            White        TD      M 19.27        3           104
    ## 18            White        TD      M 20.03        5           113
    ## 19            White        TD      M 21.03        0            96
    ## 20            White        TD      M 19.27        0            86
    ## 21            White        TD      M 19.23        0           106
    ## 22     White/Latino       ASD      M 27.37       14            65
    ## 23            White        TD      M 18.97        0            92
    ## 24      White/Asian       ASD      M 33.20       11            88
    ## 25            White        TD      M 19.10        1           115
    ## 26            White        TD      M 19.93        0           102
    ## 27            White       ASD      M 34.87       17            72
    ## 28            White        TD      F 18.93        0           106
    ## 29 African American       ASD      M 27.53       21            72
    ## 30            White       ASD      F 41.00       18            65
    ## 31            White        TD      F 18.30        0           100
    ## 32            White        TD      M 19.80        0           108
    ## 33            White        TD      M 20.10        5           102
    ## 34            White        TD      M 19.97        0            96
    ## 35            White       ASD      M 42.00       15            79
    ## 36            White        TD      M 19.37        0            96
    ## 37            White       ASD      M 33.77       10            82
    ## 38            White       ASD      M 34.03        9            82
    ## 39            White        TD      F 20.03        0           102
    ## 40            White       ASD      M 18.77        9            86
    ## 41            White       ASD      M 35.80       11            88
    ## 42            White       ASD      M 26.00       15            88
    ## 43            White        TD      M 19.23        0           102
    ## 44            White        TD      M 20.80        4           108
    ## 45            White        TD      M 19.00        0           102
    ## 46            White        TD      M 21.67        0           102
    ## 47            White        TD      F 23.50        1            88
    ## 48            White        TD      M 19.30        1           102
    ## 49     White/Latino       ASD      M 31.03        8            82
    ## 50            White        TD      M 23.07        0           108
    ## 51            White        TD      M 23.13        0           100
    ## 52            White        TD      M 22.57        0            94
    ## 53            White       ASD      M 39.50        7            70
    ## 54            White        TD      F 19.87        1           104
    ## 55            White        TD      M 20.77        0           106
    ## 56            White        TD      M 21.77        0            90
    ## 57            White       ASD      M 37.03       14            65
    ## 58            White       ASD      M 28.80       13            85
    ## 59            White       ASD      M 34.00       13            87
    ## 60            White        TD      M 23.90        0            98
    ## 61            White       ASD      M 30.40       11           100
    ## 62            White       ASD      F 31.77        0           105
    ## 63           Latino       ASD      M 35.47       19            78
    ## 64            White        TD      M 23.13        0            92
    ## 65            White        TD      M    NA        4            84
    ## 66            White        TD      M 18.07       15           104
    ##    nonVerbalIQ verbalIQ
    ## 1           28       10
    ## 2           21        9
    ## 3           19       13
    ## 4           31       13
    ## 5           13       11
    ## 6           20       17
    ## 7           25       11
    ## 8           25       11
    ## 9           22       14
    ## 10          28       10
    ## 11          17       10
    ## 12          27       11
    ## 13          27       13
    ## 14          30       16
    ## 15          21        9
    ## 16          30       20
    ## 17          27       18
    ## 18          24       20
    ## 19          26       18
    ## 20          24       15
    ## 21          21       15
    ## 22          25       19
    ## 23          23       17
    ## 24          26       19
    ## 25          24       22
    ## 26          20       16
    ## 27          26       14
    ## 28          21       19
    ## 29          22        8
    ## 30          24       14
    ## 31          24       18
    ## 32          28       14
    ## 33          32       31
    ## 34          26       17
    ## 35          27       16
    ## 36          24       19
    ## 37          27       22
    ## 38          34       27
    ## 39          27       20
    ## 40          26       14
    ## 41          28       20
    ## 42          30       24
    ## 43          25       17
    ## 44          29       22
    ## 45          25       17
    ## 46          29       26
    ## 47          29       18
    ## 48          23       21
    ## 49          31       27
    ## 50          27       27
    ## 51          27       22
    ## 52          29       22
    ## 53          33       26
    ## 54          29       28
    ## 55          29       33
    ## 56          29       22
    ## 57          42       27
    ## 58          34       27
    ## 59          30       30
    ## 60          30       30
    ## 61          32       33
    ## 62          28       33
    ## 63          26       16
    ## 64          27       17
    ## 65          17       16
    ## 66          NA       NA

USING SELECT

1.  Make a subset of the data including only kids with ASD, mlu and word
    tokens
2.  What happens if you include the name of a variable multiple times in
    a select() call?

``` r
# 1
df %>% 
  filter(Diagnosis == "ASD") %>% 
  select(CHI_MLU, totalW_CHI)
```

    ##       CHI_MLU totalW_CHI
    ## 1   2.2768595        461
    ## 2   3.4530387        562
    ## 3   3.1193182        983
    ## 4   4.3023256        674
    ## 5          NA         NA
    ## 6   3.4135021        698
    ## 7   2.0972222        269
    ## 8   2.5630252        555
    ## 9   3.5180723        490
    ## 10  3.2571429        479
    ## 11  4.0434783        539
    ## 12  3.2781955        738
    ## 13  1.3947368        197
    ## 14  2.5743590        487
    ## 15  2.5324074        468
    ## 16  2.9072581        604
    ## 17  2.4232804        404
    ## 18  2.7665198        538
    ## 19  1.0000000         29
    ## 20  0.7606838        149
    ## 21  1.1698113         62
    ## 22  1.7142857        205
    ## 23  1.6363636        349
    ## 24  1.4967320        300
    ## 25  1.2641509        130
    ## 26  1.3211679        169
    ## 27  1.3974359        210
    ## 28  1.9115646        494
    ## 29  1.5299145        194
    ## 30         NA         NA
    ## 31         NA         NA
    ## 32         NA         NA
    ## 33  1.0277778         37
    ## 34  1.0125000         82
    ## 35  1.0000000         58
    ## 36         NA         NA
    ## 37  1.0000000          9
    ## 38  1.0000000         37
    ## 39  1.3595506        227
    ## 40  1.4867257        335
    ## 41  1.9052632        330
    ## 42  1.6179402        455
    ## 43  1.5840000        197
    ## 44  2.1450382        274
    ## 45  0.1857143        214
    ## 46  1.0800000        362
    ## 47  1.6109325        503
    ## 48  2.2485380        717
    ## 49  2.2506739        792
    ## 50  2.9027778        590
    ## 51  1.0000000        122
    ## 52  1.0400000         78
    ## 53  2.0000000         96
    ## 54  0.2727273         93
    ## 55  0.2000000          5
    ## 56  0.5000000          2
    ## 57         NA         NA
    ## 58         NA         NA
    ## 59         NA         NA
    ## 60  1.4324324        103
    ## 61  1.4273504        157
    ## 62  1.0863309        148
    ## 63  1.0974026        162
    ## 64  1.4133333        197
    ## 65  1.3052632        236
    ## 66  1.0833333         26
    ## 67  1.0000000         32
    ## 68  1.0000000         40
    ## 69  1.2032520        143
    ## 70  1.0000000         34
    ## 71  1.1680000        143
    ## 72  3.4000000        483
    ## 73         NA         NA
    ## 74  3.9196891       1293
    ## 75  3.5238095        714
    ## 76  3.2919897       1154
    ## 77  3.3643411       1249
    ## 78  1.0086207        117
    ## 79  1.0114286        177
    ## 80  0.6842105         39
    ## 81  0.8947368         39
    ## 82  0.2553191        179
    ## 83  1.2883436        204
    ## 84  0.9000000         21
    ## 85  1.0265487        116
    ## 86  1.0066667        151
    ## 87  1.1250000         79
    ## 88  0.4130435        193
    ## 89  1.0843373        195
    ## 90  1.5000000          3
    ## 91  1.0000000          7
    ## 92  1.0000000          1
    ## 93  1.0000000         14
    ## 94  1.0000000         33
    ## 95  2.6315789        100
    ## 96  1.2500000         35
    ## 97  1.0000000         10
    ## 98  1.0833333         26
    ## 99  1.2100840        142
    ## 100 0.6756757         42
    ## 101 0.7536232         79
    ## 102 1.3766234        180
    ## 103 1.9481132        368
    ## 104 1.8056872        365
    ## 105 2.1962617        438
    ## 106 2.5758929        537
    ## 107 2.2745098        605
    ## 108 0.4805825        337
    ## 109 1.3432836        433
    ## 110 1.8812665        694
    ## 111 2.5159817        510
    ## 112 2.7468750        815
    ## 113 3.0774194        897
    ## 114 1.2043011        109
    ## 115 1.2571429        126
    ## 116 1.8880000        232
    ## 117 1.7000000        135
    ## 118 1.6511628        340
    ## 119 1.6447368        110
    ## 120 1.0175439         58
    ## 121 1.2195122        148
    ## 122 2.0704225        404
    ## 123 2.9900000        493
    ## 124        NA         NA
    ## 125 2.1586207        571
    ## 126 1.9144981        450
    ## 127 1.9832215        536
    ## 128 2.3053892       1023
    ## 129 2.6837838        822
    ## 130 2.8665049       1054
    ## 131 2.6794872        921
    ## 132 1.1666667         63
    ## 133 1.2258065         38
    ## 134 1.0000000          3
    ## 135 1.1200000         55
    ## 136 1.0000000         32
    ## 137 1.1610169        137
    ## 138 1.2500000         40
    ## 139 1.0000000          4
    ## 140 1.0000000         45
    ## 141 1.4102564        255
    ## 142 1.4734513        306
    ## 143 1.1000000         43
    ## 144 1.3628319        147
    ## 145 1.1666667         77
    ## 146 1.0000000         70
    ## 147 0.0000000          0
    ## 148 1.3333333          8
    ## 149 1.0444444         47
    ## 150 1.0000000         19
    ## 151 1.7500000         14
    ## 152 1.0588235         36
    ## 153 0.0000000          0
    ## 154 1.0000000         44
    ## 155 0.1621622         41
    ## 156 0.0000000          1
    ## 157 0.2000000         30
    ## 158 0.0156250         64
    ## 159 1.1809045        233
    ## 160 1.4432990        260
    ## 161 1.8000000        393
    ## 162 2.0127796        574
    ## 163 1.7591463        531
    ## 164 2.1553398        618
    ## 165 2.8763441        469
    ## 166 2.7840000        670
    ## 167 4.1318681        698
    ## 168 3.3598326        693
    ## 169 2.9655172        357
    ## 170 3.4415584        713
    ## 171 1.0375000        166
    ## 172 1.0990991        356
    ## 173 1.5828571        254
    ## 174 1.3204225        334
    ## 175 1.6688963        511
    ## 176 1.4012346        210

``` r
# 2 - nothing happens
df %>% 
  select(CHI_MLU, totalW_CHI, CHI_MLU, CHI_MLU )
```

    ##       CHI_MLU totalW_CHI
    ## 1   1.2522523        139
    ## 2   1.0136054        148
    ## 3   1.5568862        255
    ## 4   2.2515723        321
    ## 5   3.2380952        472
    ## 6   2.8651685        686
    ## 7   2.2768595        461
    ## 8   3.4530387        562
    ## 9   3.1193182        983
    ## 10  4.3023256        674
    ## 11         NA         NA
    ## 12  3.4135021        698
    ## 13  1.8776978        260
    ## 14  2.6418605        530
    ## 15  2.6916300        542
    ## 16  3.9292035        754
    ## 17  3.2985782        588
    ## 18  3.7103448        460
    ## 19  2.0972222        269
    ## 20  2.5630252        555
    ## 21  3.5180723        490
    ## 22  3.2571429        479
    ## 23  4.0434783        539
    ## 24  3.2781955        738
    ## 25  1.3947368        197
    ## 26  2.5743590        487
    ## 27  2.5324074        468
    ## 28  2.9072581        604
    ## 29  2.4232804        404
    ## 30  2.7665198        538
    ## 31  1.0000000         29
    ## 32  0.7606838        149
    ## 33  1.1698113         62
    ## 34  1.7142857        205
    ## 35  1.6363636        349
    ## 36  1.4967320        300
    ## 37  1.2641509        130
    ## 38  1.3211679        169
    ## 39  1.3974359        210
    ## 40  1.9115646        494
    ## 41  1.5299145        194
    ## 42         NA         NA
    ## 43  1.5600000        143
    ## 44  1.6019108        554
    ## 45         NA         NA
    ## 46  3.0265957        493
    ## 47  1.9408602        323
    ## 48  2.2258065        197
    ## 49  1.0378788        137
    ## 50  1.6162791        132
    ## 51  1.8320312        453
    ## 52  2.7756410        390
    ## 53  2.8358209        346
    ## 54         NA         NA
    ## 55  1.0375000         83
    ## 56  1.1200000         55
    ## 57  1.5520000        188
    ## 58  1.6479401        414
    ## 59  2.2835821        431
    ## 60  2.7051282        189
    ## 61         NA         NA
    ## 62         NA         NA
    ## 63  1.2168675        101
    ## 64  1.0630631        117
    ## 65  1.5234375        193
    ## 66  2.1348315        354
    ## 67  2.5482456        513
    ## 68  2.7571429        666
    ## 69  1.0877193         62
    ## 70  1.3647799        203
    ## 71  3.1857708        733
    ## 72  3.0000000        243
    ## 73  4.3647541        916
    ## 74  3.5049505        640
    ## 75  1.5740741        249
    ## 76  1.4761905        393
    ## 77  2.0798817        630
    ## 78  2.7982833        538
    ## 79  3.2301136        932
    ## 80  3.7011952        864
    ## 81  1.0396040        105
    ## 82  1.0744681        297
    ## 83  1.1855072        368
    ## 84  2.3178295        557
    ## 85  3.3750000        755
    ## 86  3.8114035        793
    ## 87  1.1647059         99
    ## 88  1.0460526        159
    ## 89  1.4408602        252
    ## 90  2.5613208        495
    ## 91  2.4691358        160
    ## 92  2.8690476        410
    ## 93  1.5988372        254
    ## 94  2.7441077        733
    ## 95  2.8076923        825
    ## 96  2.3225806        793
    ## 97  3.1095238        583
    ## 98  2.9482072        710
    ## 99  1.0277778         37
    ## 100 1.0125000         82
    ## 101 1.0000000         58
    ## 102        NA         NA
    ## 103 1.0000000          9
    ## 104 1.0000000         37
    ## 105 1.7948718        406
    ## 106 2.7220339        738
    ## 107 3.3400000        940
    ## 108 3.2128205       1092
    ## 109 3.0902778        769
    ## 110 2.9095355       1079
    ## 111 1.3595506        227
    ## 112 1.4867257        335
    ## 113 1.9052632        330
    ## 114 1.6179402        455
    ## 115 1.5840000        197
    ## 116 2.1450382        274
    ## 117 0.1857143        214
    ## 118 1.0800000        362
    ## 119 1.6109325        503
    ## 120 2.2485380        717
    ## 121 2.2506739        792
    ## 122 2.9027778        590
    ## 123 1.0000000        122
    ## 124 1.0400000         78
    ## 125 2.0000000         96
    ## 126 0.2727273         93
    ## 127 0.2000000          5
    ## 128 0.5000000          2
    ## 129        NA         NA
    ## 130        NA         NA
    ## 131        NA         NA
    ## 132 1.2371134        118
    ## 133 1.7804878        414
    ## 134 1.4549550        305
    ## 135 2.3740741        592
    ## 136 3.7000000        686
    ## 137 3.0911950        847
    ## 138 1.4324324        103
    ## 139 1.4273504        157
    ## 140 1.0863309        148
    ## 141 1.0974026        162
    ## 142 1.4133333        197
    ## 143 1.3052632        236
    ## 144 1.0833333         26
    ## 145 1.0000000         32
    ## 146 1.0000000         40
    ## 147 1.2032520        143
    ## 148 1.0000000         34
    ## 149 1.1680000        143
    ## 150 1.5827338        433
    ## 151 2.4847328        623
    ## 152 2.8042169        826
    ## 153 3.7310924       1225
    ## 154 2.7413793       1145
    ## 155 3.0614525       1010
    ## 156 3.4000000        483
    ## 157        NA         NA
    ## 158 3.9196891       1293
    ## 159 3.5238095        714
    ## 160 3.2919897       1154
    ## 161 3.3643411       1249
    ## 162 1.3661972         95
    ## 163 1.4228571        236
    ## 164 1.4976077        299
    ## 165 1.7405858        384
    ## 166 2.1791908        318
    ## 167 2.4800000        530
    ## 168 1.2758621        111
    ## 169 1.6557377        258
    ## 170 1.8369099        403
    ## 171 2.3500000        502
    ## 172 3.5852535        622
    ## 173 2.9607843        521
    ## 174 1.0086207        117
    ## 175 1.0114286        177
    ## 176 0.6842105         39
    ## 177 0.8947368         39
    ## 178 0.2553191        179
    ## 179 1.2883436        204
    ## 180 0.9000000         21
    ## 181 1.0265487        116
    ## 182 1.0066667        151
    ## 183 1.1250000         79
    ## 184 0.4130435        193
    ## 185 1.0843373        195
    ## 186 1.5333333        319
    ## 187 2.2481481        542
    ## 188 2.9875260       1246
    ## 189 2.7272727        750
    ## 190 2.1420613        690
    ## 191 2.0717489        418
    ## 192 1.5000000          3
    ## 193 1.0000000          7
    ## 194 1.0000000          1
    ## 195 1.0000000         14
    ## 196 1.0000000         33
    ## 197 2.6315789        100
    ## 198 1.2500000         35
    ## 199 1.0000000         10
    ## 200 1.0833333         26
    ## 201 1.2100840        142
    ## 202 0.6756757         42
    ## 203 0.7536232         79
    ## 204 1.3766234        180
    ## 205 1.9481132        368
    ## 206 1.8056872        365
    ## 207 2.1962617        438
    ## 208 2.5758929        537
    ## 209 2.2745098        605
    ## 210 1.2641509        260
    ## 211 1.9342916        879
    ## 212 2.5710145        702
    ## 213 1.9743590         74
    ## 214 2.5541796        736
    ## 215 2.3815789        611
    ## 216 1.2307692         16
    ## 217 1.3103448        107
    ## 218 1.7500000        218
    ## 219 2.2594142        477
    ## 220 2.2787611        459
    ## 221 2.5614754        568
    ## 222 0.4805825        337
    ## 223 1.3432836        433
    ## 224 1.8812665        694
    ## 225 2.5159817        510
    ## 226 2.7468750        815
    ## 227 3.0774194        897
    ## 228 1.3322785        398
    ## 229 2.1727273        439
    ## 230 2.8690476        825
    ## 231 2.6475410        576
    ## 232 3.5185185        800
    ## 233        NA         NA
    ## 234 1.2043011        109
    ## 235 1.2571429        126
    ## 236 1.8880000        232
    ## 237 1.7000000        135
    ## 238 1.6511628        340
    ## 239 1.6447368        110
    ## 240 1.0175439         58
    ## 241 1.2195122        148
    ## 242 2.0704225        404
    ## 243 2.9900000        493
    ## 244        NA         NA
    ## 245 2.1586207        571
    ## 246 1.0262009        235
    ## 247 1.1974249        264
    ## 248 1.9086294        352
    ## 249 2.2325581        434
    ## 250 2.2892308        651
    ## 251 2.8526316        702
    ## 252 1.3034483        176
    ## 253 2.0046729        423
    ## 254 2.4911032        604
    ## 255 3.8300395        820
    ## 256 3.7749077        875
    ## 257 3.0727969        719
    ## 258 1.9144981        450
    ## 259 1.9832215        536
    ## 260 2.3053892       1023
    ## 261 2.6837838        822
    ## 262 2.8665049       1054
    ## 263 2.6794872        921
    ## 264 1.2592593         68
    ## 265 1.3043478        120
    ## 266 2.2313433        528
    ## 267 2.5370370        378
    ## 268 3.2000000        433
    ## 269        NA         NA
    ## 270 0.5584416         91
    ## 271 0.9051724        165
    ## 272 1.1722689        354
    ## 273 1.7600000        433
    ## 274 2.2558140        502
    ## 275        NA         NA
    ## 276        NA         NA
    ## 277 1.1666667         63
    ## 278 1.2258065         38
    ## 279 1.0000000          3
    ## 280 1.1200000         55
    ## 281 1.0000000         32
    ## 282 1.1610169        137
    ## 283        NA         NA
    ## 284        NA         NA
    ## 285 1.0761421        212
    ## 286 2.2481481        571
    ## 287 2.2893617        486
    ## 288 3.1622419        973
    ## 289 2.7388060        304
    ## 290 2.7600000         61
    ## 291 1.2500000         40
    ## 292 1.0000000          4
    ## 293 1.0000000         45
    ## 294 1.4102564        255
    ## 295 1.4734513        306
    ## 296 1.4258373        286
    ## 297 2.4967742        356
    ## 298 3.1124498        671
    ## 299 3.4809524        684
    ## 300 3.5371429        586
    ## 301 3.5950000        646
    ## 302 1.1000000         43
    ## 303 1.3628319        147
    ## 304 1.1666667         77
    ## 305 1.0000000         70
    ## 306 0.0000000          0
    ## 307 1.3333333          8
    ## 308 1.1818182         38
    ## 309 1.1976744         97
    ## 310 1.5529412        122
    ## 311 3.0289855        348
    ## 312 2.9215686        238
    ## 313 2.7612903        395
    ## 314 1.4086957        319
    ## 315 1.7359551        298
    ## 316 3.3037543        897
    ## 317 3.0775510        642
    ## 318 2.8321678        723
    ## 319 2.4292929        436
    ## 320 1.9397163        473
    ## 321 3.2179487        449
    ## 322 3.1313559        660
    ## 323 3.6340694        978
    ## 324 3.8225806        814
    ## 325 3.2439024        358
    ## 326 1.3139535        109
    ## 327 2.0466667        287
    ## 328 2.3309353        261
    ## 329 3.4065041        694
    ## 330 3.6072874        725
    ## 331 2.8921569        517
    ## 332 1.1846154        154
    ## 333 1.7902098        244
    ## 334 2.4932432        672
    ## 335 2.8620690       1051
    ## 336        NA         NA
    ## 337 2.9092742       1294
    ## 338 1.0444444         47
    ## 339 1.0000000         19
    ## 340 1.7500000         14
    ## 341 1.0588235         36
    ## 342 0.0000000          0
    ## 343 1.0000000         44
    ## 344 0.1621622         41
    ## 345 0.0000000          1
    ## 346 0.2000000         30
    ## 347 0.0156250         64
    ## 348 1.1809045        233
    ## 349 1.4432990        260
    ## 350 1.8000000        393
    ## 351 2.0127796        574
    ## 352 1.7591463        531
    ## 353 2.1553398        618
    ## 354 1.1830065        244
    ## 355 1.7709251        401
    ## 356 2.1157895        424
    ## 357 2.7148289        637
    ## 358 2.6038462        636
    ## 359 2.8480000        659
    ## 360 2.8763441        469
    ## 361 2.7840000        670
    ## 362 4.1318681        698
    ## 363 3.3598326        693
    ## 364 2.9655172        357
    ## 365 3.4415584        713
    ## 366 1.0375000        166
    ## 367 1.0990991        356
    ## 368 1.5828571        254
    ## 369 1.3204225        334
    ## 370 1.6688963        511
    ## 371 1.4012346        210
    ## 372        NA         NA

USING MUTATE, SUMMARISE and PIPES 1. Add a column to the data set that
represents the mean number of words spoken during all visits. 2. Use the
summarise function and pipes to add an column in the data set containing
the mean amount of words produced by each trial across all visits. HINT:
group by Child.ID 3. The solution to task above enables us to assess the
average amount of words produced by each child. Why don’t we just use
these average values to describe the language production of the
children? What is the advantage of keeping all the data?

``` r
# 1
df %>%
  mutate(mtoal = mean(totalW_CHI, na.rm = T))
```

    ##     SUBJ severity1 Socialization1 nonVerbalIQ1 verbalIQ1 VISIT  MOT_MLU
    ## 1      1         0            108           28        14     1 3.621993
    ## 2      1         0            108           28        14     2 3.857367
    ## 3      1         0            108           28        14     3 4.321881
    ## 4      1         0            108           28        14     4 4.415330
    ## 5      1         0            108           28        14     5 5.209615
    ## 6      1         0            108           28        14     6 4.664013
    ## 7      2        13             85           34        27     1 4.098446
    ## 8      2        13             85           34        27     2 4.964664
    ## 9      2        13             85           34        27     3 4.147059
    ## 10     2        13             85           34        27     4 5.309804
    ## 11     2        13             85           34        27     5       NA
    ## 12     2        13             85           34        27     6 4.588477
    ## 13     3         1             88           29        18     1 3.757269
    ## 14     3         1             88           29        18     2 4.572086
    ## 15     3         1             88           29        18     3 5.134892
    ## 16     3         1             88           29        18     4 5.301053
    ## 17     3         1             88           29        18     5 4.566038
    ## 18     3         1             88           29        18     6 5.229885
    ## 19     4         8             82           31        27     1 3.459370
    ## 20     4         8             82           31        27     2 4.130337
    ## 21     4         8             82           31        27     3 3.818681
    ## 22     4         8             82           31        27     4 4.301624
    ## 23     4         8             82           31        27     5 4.602851
    ## 24     4         8             82           31        27     6 3.532374
    ## 25     5         9             82           34        27     1 3.986357
    ## 26     5         9             82           34        27     2 4.886646
    ## 27     5         9             82           34        27     3 4.198973
    ## 28     5         9             82           34        27     4 4.744000
    ## 29     5         9             82           34        27     5 4.292135
    ## 30     5         9             82           34        27     6 4.587179
    ## 31     6        17             68           20        17     1 2.618729
    ## 32     6        17             68           20        17     2 1.995885
    ## 33     6        17             68           20        17     3 1.856115
    ## 34     6        17             68           20        17     4 2.035714
    ## 35     6        17             68           20        17     5 1.948454
    ## 36     6        17             68           20        17     6 2.483146
    ## 37     7        18             65           24        14     1 2.244755
    ## 38     7        18             65           24        14     2 2.417344
    ## 39     7        18             65           24        14     3 2.633929
    ## 40     7        18             65           24        14     4 2.736842
    ## 41     7        18             65           24        14     5 3.992095
    ## 42     7        18             65           24        14     6       NA
    ## 43     8         5            102           32        31     1 3.265082
    ## 44     8         5            102           32        31     2 3.967213
    ## 45     8         5            102           32        31     3       NA
    ## 46     8         5            102           32        31     4 4.658333
    ## 47     8         5            102           32        31     5 4.564103
    ## 48     8         5            102           32        31     6 4.366972
    ## 49     9         0            100           24        18     1 3.544419
    ## 50     9         0            100           24        18     2 4.079060
    ## 51     9         0            100           24        18     3 4.592593
    ## 52     9         0            100           24        18     4 4.262195
    ## 53     9         0            100           24        18     5 4.384946
    ## 54     9         0            100           24        18     6       NA
    ## 55    10         3            104           27        18     1 4.204846
    ## 56    10         3            104           27        18     2 4.378840
    ## 57    10         3            104           27        18     3 4.298942
    ## 58    10         3            104           27        18     4 3.790896
    ## 59    10         3            104           27        18     5 3.867601
    ## 60    10         3            104           27        18     6 4.235585
    ## 61    11         0            105           28        33     1       NA
    ## 62    11         0            105           28        33     2       NA
    ## 63    12         0            106           21        15     1 3.380463
    ## 64    12         0            106           21        15     2 2.821918
    ## 65    12         0            106           21        15     3 3.940711
    ## 66    12         0            106           21        15     4 4.732733
    ## 67    12         0            106           21        15     5 4.543210
    ## 68    12         0            106           21        15     6 4.287582
    ## 69    13         0            104           30        16     1 4.195335
    ## 70    13         0            104           30        16     2 3.444664
    ## 71    13         0            104           30        16     3 4.974684
    ## 72    13         0            104           30        16     4 3.988304
    ## 73    13         0            104           30        16     5 4.910494
    ## 74    13         0            104           30        16     6 4.468493
    ## 75    14         0            102           25        17     1 3.960254
    ## 76    14         0            102           25        17     2 3.817109
    ## 77    14         0            102           25        17     3 3.871968
    ## 78    14         0            102           25        17     4 4.083700
    ## 79    14         0            102           25        17     5 4.487842
    ## 80    14         0            102           25        17     6 4.847418
    ## 81    15         0             92           23        17     1 3.420315
    ## 82    15         0             92           23        17     2 3.239832
    ## 83    15         0             92           23        17     3 3.848175
    ## 84    15         0             92           23        17     4 3.801292
    ## 85    15         0             92           23        17     5 4.446281
    ## 86    15         0             92           23        17     6 4.664286
    ## 87    16         0             86           24        15     1 3.967078
    ## 88    16         0             86           24        15     2 4.224839
    ## 89    16         0             86           24        15     3 4.182979
    ## 90    16         0             86           24        15     4 4.584071
    ## 91    16         0             86           24        15     5 4.165414
    ## 92    16         0             86           24        15     6 4.347709
    ## 93    17         0            102           29        26     1 4.910190
    ## 94    17         0            102           29        26     2 4.750455
    ## 95    17         0            102           29        26     3 4.164789
    ## 96    17         0            102           29        26     4 4.251605
    ## 97    17         0            102           29        26     5 5.433579
    ## 98    17         0            102           29        26     6 4.445872
    ## 99    18        14             74           25        11     1 3.182390
    ## 100   18        14             74           25        11     2 2.947230
    ## 101   18        14             74           25        11     3 2.919169
    ## 102   18        14             74           25        11     4       NA
    ## 103   18        14             74           25        11     5 3.996071
    ## 104   18        14             74           25        11     6 3.650235
    ## 105   19         0            106           29        33     1 3.587855
    ## 106   19         0            106           29        33     2 4.357911
    ## 107   19         0            106           29        33     3 4.116057
    ## 108   19         0            106           29        33     4 4.131579
    ## 109   19         0            106           29        33     5 3.877102
    ## 110   19         0            106           29        33     6 4.013353
    ## 111   20        11             88           28        20     1 2.539823
    ## 112   20        11             88           28        20     2 3.170000
    ## 113   20        11             88           28        20     3 3.615176
    ## 114   20        11             88           28        20     4 2.762463
    ## 115   20        11             88           28        20     5 4.157191
    ## 116   20        11             88           28        20     6 3.156695
    ## 117   21         9             86           26        14     1 2.524740
    ## 118   21         9             86           26        14     2 2.841004
    ## 119   21         9             86           26        14     3 4.063518
    ## 120   21         9             86           26        14     4 4.560201
    ## 121   21         9             86           26        14     5 4.837884
    ## 122   21         9             86           26        14     6 5.379798
    ## 123   22        21             72           22         8     1 4.390879
    ## 124   22        21             72           22         8     2 3.381510
    ## 125   22        21             72           22         8     3 3.646778
    ## 126   22        21             72           22         8     4 3.600624
    ## 127   22        21             72           22         8     5 4.019231
    ## 128   22        21             72           22         8     6 3.943636
    ## 129   23        19             78           26        16     1       NA
    ## 130   23        19             78           26        16     2       NA
    ## 131   23        19             78           26        16     3       NA
    ## 132   24         0            106           21        19     1 3.630476
    ## 133   24         0            106           21        19     2 3.978599
    ## 134   24         0            106           21        19     3 3.094880
    ## 135   24         0            106           21        19     4 3.991667
    ## 136   24         0            106           21        19     5 4.746606
    ## 137   24         0            106           21        19     6 4.211321
    ## 138   25        14             65           25        19     1 3.024548
    ## 139   25        14             65           25        19     2 2.903723
    ## 140   25        14             65           25        19     3 3.215859
    ## 141   25        14             65           25        19     4 3.012594
    ## 142   25        14             65           25        19     5 2.822259
    ## 143   25        14             65           25        19     6 3.534173
    ## 144   26        20             67           13        11     1 2.917355
    ## 145   26        20             67           13        11     2 3.815141
    ## 146   26        20             67           13        11     3 3.910448
    ## 147   26        20             67           13        11     4 3.814815
    ## 148   26        20             67           13        11     5 4.421222
    ## 149   26        20             67           13        11     6 3.860795
    ## 150   27         0             90           29        22     1 4.643200
    ## 151   27         0             90           29        22     2 4.600000
    ## 152   27         0             90           29        22     3 4.127941
    ## 153   27         0             90           29        22     4 5.362500
    ## 154   27         0             90           29        22     5 4.267409
    ## 155   27         0             90           29        22     6 4.472993
    ## 156   28        11            100           32        33     1 4.690751
    ## 157   28        11            100           32        33     2       NA
    ## 158   28        11            100           32        33     3 4.316279
    ## 159   28        11            100           32        33     4 4.857143
    ## 160   28        11            100           32        33     5 4.345515
    ## 161   28        11            100           32        33     6 4.111413
    ## 162   29         0             96           26        18     1 3.616867
    ## 163   29         0             96           26        18     2 3.869654
    ## 164   29         0             96           26        18     3 3.846626
    ## 165   29         0             96           26        18     4 3.973469
    ## 166   29         0             96           26        18     5 3.479393
    ## 167   29         0             96           26        18     6 4.254505
    ## 168   30         0            102           20        16     1 2.788793
    ## 169   30         0            102           20        16     2 3.367292
    ## 170   30         0            102           20        16     3 3.210084
    ## 171   30         0            102           20        16     4 3.683406
    ## 172   30         0            102           20        16     5 3.708934
    ## 173   30         0            102           20        16     6 4.239437
    ## 174   31        17             72           26        14     1 3.304189
    ## 175   31        17             72           26        14     2 4.003072
    ## 176   31        17             72           26        14     3 4.382550
    ## 177   31        17             72           26        14     4 3.885113
    ## 178   31        17             72           26        14     5 4.872014
    ## 179   31        17             72           26        14     6 4.349353
    ## 180   32        12             70           31        13     1 3.607088
    ## 181   32        12             70           31        13     2 3.843411
    ## 182   32        12             70           31        13     3 4.015699
    ## 183   32        12             70           31        13     4 3.961832
    ## 184   32        12             70           31        13     5 3.859624
    ## 185   32        12             70           31        13     6 4.341549
    ## 186   33         0            100           27        22     1 3.494327
    ## 187   33         0            100           27        22     2 4.222034
    ## 188   33         0            100           27        22     3 3.552459
    ## 189   33         0            100           27        22     4 3.667638
    ## 190   33         0            100           27        22     5 3.906856
    ## 191   33         0            100           27        22     6 3.603473
    ## 192   34        21             69           21         9     1 3.686347
    ## 193   34        21             69           21         9     2 4.142562
    ## 194   34        21             69           21         9     3 3.990826
    ## 195   34        21             69           21         9     4 3.743333
    ## 196   34        21             69           21         9     5 3.414894
    ## 197   34        21             69           21         9     6 4.003257
    ## 198   35        14             76           25        11     1 2.287293
    ## 199   35        14             76           25        11     2 2.879925
    ## 200   35        14             76           25        11     3 3.404959
    ## 201   35        14             76           25        11     4 3.110932
    ## 202   35        14             76           25        11     5 4.185417
    ## 203   35        14             76           25        11     6 3.965392
    ## 204   36        10             82           27        22     1 2.743455
    ## 205   36        10             82           27        22     2 2.667722
    ## 206   36        10             82           27        22     3 3.866834
    ## 207   36        10             82           27        22     4 3.997126
    ## 208   36        10             82           27        22     5 3.419664
    ## 209   36        10             82           27        22     6 3.370937
    ## 210   37         1            102           23        21     1 3.561364
    ## 211   37         1            102           23        21     2 3.636519
    ## 212   37         1            102           23        21     3 3.675134
    ## 213   37         1            102           23        21     4 4.001613
    ## 214   37         1            102           23        21     5 4.069659
    ## 215   37         1            102           23        21     6 4.239198
    ## 216   38         3             98           19        13     1 3.921109
    ## 217   38         3             98           19        13     2 3.942164
    ## 218   38         3             98           19        13     3 4.595568
    ## 219   38         3             98           19        13     4 3.705441
    ## 220   38         3             98           19        13     5 4.137405
    ## 221   38         3             98           19        13     6 4.388235
    ## 222   39         7             70           33        26     1 4.135036
    ## 223   39         7             70           33        26     2 4.054054
    ## 224   39         7             70           33        26     3 3.642412
    ## 225   39         7             70           33        26     4 5.050000
    ## 226   39         7             70           33        26     5 4.658802
    ## 227   39         7             70           33        26     6 4.240798
    ## 228   40         1            104           29        28     1 3.420975
    ## 229   40         1            104           29        28     2 4.073282
    ## 230   40         1            104           29        28     3 4.577491
    ## 231   40         1            104           29        28     4 4.169162
    ## 232   40         1            104           29        28     5 4.917927
    ## 233   40         1            104           29        28     6       NA
    ## 234   41        11             88           26        19     1 3.298748
    ## 235   41        11             88           26        19     2 2.760976
    ## 236   41        11             88           26        19     3 3.607664
    ## 237   41        11             88           26        19     4 3.714556
    ## 238   41        11             88           26        19     5 2.902089
    ## 239   41        11             88           26        19     6 4.100707
    ## 240   42        13             86           27        13     1 2.997050
    ## 241   42        13             86           27        13     2 2.900838
    ## 242   42        13             86           27        13     3 3.723664
    ## 243   42        13             86           27        13     4 3.812930
    ## 244   42        13             86           27        13     5       NA
    ## 245   42        13             86           27        13     6 3.926928
    ## 246   43         0            102           25        17     1 3.093146
    ## 247   43         0            102           25        17     2 3.751332
    ## 248   43         0            102           25        17     3 4.050505
    ## 249   43         0            102           25        17     4 4.118790
    ## 250   43         0            102           25        17     5 3.969147
    ## 251   43         0            102           25        17     6 4.061475
    ## 252   44         0             96           24        19     1 4.033333
    ## 253   44         0             96           24        19     2 4.100167
    ## 254   44         0             96           24        19     3 4.057047
    ## 255   44         0             96           24        19     4 4.611247
    ## 256   44         0             96           24        19     5 3.921444
    ## 257   44         0             96           24        19     6 3.391525
    ## 258   45        14             65           42        27     1 3.341418
    ## 259   45        14             65           42        27     2 3.505582
    ## 260   45        14             65           42        27     3 3.573574
    ## 261   45        14             65           42        27     4 3.905213
    ## 262   45        14             65           42        27     5 4.232258
    ## 263   45        14             65           42        27     6 4.250853
    ## 264   46         3             94           30        20     1 3.088757
    ## 265   46         3             94           30        20     2 3.445545
    ## 266   46         3             94           30        20     3 4.106212
    ## 267   46         3             94           30        20     4 4.256790
    ## 268   46         3             94           30        20     5 4.113846
    ## 269   46         3             94           30        20     6       NA
    ## 270   47         5            113           24        20     1 2.776181
    ## 271   47         5            113           24        20     2 3.171717
    ## 272   47         5            113           24        20     3 3.395899
    ## 273   47         5            113           24        20     4 4.019928
    ## 274   47         5            113           24        20     5 4.329810
    ## 275   48         0             92           27        17     1       NA
    ## 276   48         0             92           27        17     2       NA
    ## 277   49        20             75           21         9     1 4.883966
    ## 278   49        20             75           21         9     2 3.830795
    ## 279   49        20             75           21         9     3 2.821782
    ## 280   49        20             75           21         9     4 2.377171
    ## 281   49        20             75           21         9     5 3.637965
    ## 282   49        20             75           21         9     6 3.460548
    ## 283   50         4             84           17        16     1       NA
    ## 284   50         4             84           17        16     2       NA
    ## 285   51         0            102           27        20     1 3.943005
    ## 286   51         0            102           27        20     2 4.183445
    ## 287   51         0            102           27        20     3 3.960265
    ## 288   51         0            102           27        20     4 4.190698
    ## 289   51         0            102           27        20     5 3.673418
    ## 290   51         0            102           27        20     6 4.676101
    ## 291   52        17             82           28        10     1 3.765528
    ## 292   52        17             82           28        10     2 3.555804
    ## 293   52        17             82           28        10     4 3.339114
    ## 294   52        17             82           28        10     5 3.162554
    ## 295   52        17             82           28        10     6 3.370093
    ## 296   53         0            108           27        27     1 4.030075
    ## 297   53         0            108           27        27     2 4.745238
    ## 298   53         0            108           27        27     3 4.737127
    ## 299   53         0            108           27        27     4 4.880240
    ## 300   53         0            108           27        27     5 5.743772
    ## 301   53         0            108           27        27     6 5.247093
    ## 302   54        19             64           17        10     1 3.704110
    ## 303   54        19             64           17        10     2 3.563356
    ## 304   54        19             64           17        10     3 3.979950
    ## 305   54        19             64           17        10     4 4.009934
    ## 306   54        19             64           17        10     5 3.864407
    ## 307   54        19             64           17        10     6 3.886842
    ## 308   55         1            102           22        14     1 3.435743
    ## 309   55         1            102           22        14     2 4.073350
    ## 310   55         1            102           22        14     3 4.022624
    ## 311   55         1            102           22        14     4 4.137014
    ## 312   55         1            102           22        14     5 5.185941
    ## 313   55         1            102           22        14     6 5.153639
    ## 314   56         0             94           29        22     1 5.344227
    ## 315   56         0             94           29        22     2 4.567329
    ## 316   56         0             94           29        22     3 5.288991
    ## 317   56         0             94           29        22     4 5.338462
    ## 318   56         0             94           29        22     5 4.983389
    ## 319   56         0             94           29        22     6 5.587332
    ## 320   57         0             98           30        30     1 4.159159
    ## 321   57         0             98           30        30     2 4.557471
    ## 322   57         0             98           30        30     3 4.078292
    ## 323   57         0             98           30        30     4 4.458937
    ## 324   57         0             98           30        30     5 4.857143
    ## 325   57         0             98           30        30     6 3.706790
    ## 326   58         1            115           24        22     1 3.487871
    ## 327   58         1            115           24        22     2 3.609881
    ## 328   58         1            115           24        22     3 4.298667
    ## 329   58         1            115           24        22     4 3.819961
    ## 330   58         1            115           24        22     5 3.750000
    ## 331   58         1            115           24        22     6 4.186161
    ## 332   59         0             96           26        17     1 3.509138
    ## 333   59         0             96           26        17     2 3.789634
    ## 334   59         0             96           26        17     3 4.231362
    ## 335   59         0             96           26        17     4 4.177340
    ## 336   59         0             96           26        17     5       NA
    ## 337   59         0             96           26        17     6 3.957230
    ## 338   60        14             77           27        11     1 2.548969
    ## 339   60        14             77           27        11     2 3.430642
    ## 340   60        14             77           27        11     3 2.963303
    ## 341   60        14             77           27        11     6 3.474725
    ## 342   61        15             66           28        10     1 3.833770
    ## 343   61        15             66           28        10     2 3.688478
    ## 344   61        15             66           28        10     3 4.145588
    ## 345   61        15             66           28        10     4 3.536415
    ## 346   61        15             66           28        10     5 3.693846
    ## 347   61        15             66           28        10     6 3.241422
    ## 348   62        15             88           30        24     1 2.747100
    ## 349   62        15             88           30        24     2 3.372320
    ## 350   62        15             88           30        24     3 3.774030
    ## 351   62        15             88           30        24     4 3.471510
    ## 352   62        15             88           30        24     5 3.290683
    ## 353   62        15             88           30        24     6 3.320000
    ## 354   63         4            108           29        22     1 3.432387
    ## 355   63         4            108           29        22     2 4.001724
    ## 356   63         4            108           29        22     3 4.210407
    ## 357   63         4            108           29        22     4 4.697624
    ## 358   63         4            108           29        22     5 4.424837
    ## 359   63         4            108           29        22     6 4.113158
    ## 360   64        13             87           30        30     1 3.604140
    ## 361   64        13             87           30        30     2 4.604341
    ## 362   64        13             87           30        30     3 4.907591
    ## 363   64        13             87           30        30     4 4.085409
    ## 364   64        13             87           30        30     5 4.223572
    ## 365   64        13             87           30        30     6 4.080446
    ## 366   65        15             79           27        16     1 3.030405
    ## 367   65        15             79           27        16     2 3.302663
    ## 368   65        15             79           27        16     3 4.520325
    ## 369   65        15             79           27        16     4 3.312388
    ## 370   65        15             79           27        16     5 2.965928
    ## 371   65        15             79           27        16     6 3.514403
    ## 372   66        15            104           NA        NA     1       NA
    ##       CHI_MLU uniqueW_MOT uniqueW_CHI totalW_MOT totalW_CHI
    ## 1   1.2522523         378          14       1835        139
    ## 2   1.0136054         403          18       2160        148
    ## 3   1.5568862         455          97       2149        255
    ## 4   2.2515723         533         133       2260        321
    ## 5   3.2380952         601         182       2553        472
    ## 6   2.8651685         595         210       2586        686
    ## 7   2.2768595         317         146       1428        461
    ## 8   3.4530387         307         171       1270        562
    ## 9   3.1193182         351         262       1445        983
    ## 10  4.3023256         335         200       1286        674
    ## 11         NA          NA          NA         NA         NA
    ## 12  3.4135021         304         245        999        698
    ## 13  1.8776978         334          51       2674        260
    ## 14  2.6418605         464         149       2694        530
    ## 15  2.6916300         482         164       2630        542
    ## 16  3.9292035         449         206       2397        754
    ## 17  3.2985782         534         207       2672        588
    ## 18  3.7103448         486         173       2564        460
    ## 19  2.0972222         379         102       2009        269
    ## 20  2.5630252         343         170       1657        555
    ## 21  3.5180723         388         165       1788        490
    ## 22  3.2571429         356         163       1711        479
    ## 23  4.0434783         397         146       2082        539
    ## 24  3.2781955         410         166       2171        738
    ## 25  1.3947368         324          57       2859        197
    ## 26  2.5743590         441         152       2955        487
    ## 27  2.5324074         412         159       2939        468
    ## 28  2.9072581         384         187       2685        604
    ## 29  2.4232804         418         144       2749        404
    ## 30  2.7665198         462         179       3182        538
    ## 31  1.0000000         212           4        761         29
    ## 32  0.7606838         140          29        464        149
    ## 33  1.1698113          74          12        250         62
    ## 34  1.7142857         125          50        360        205
    ## 35  1.6363636          92          80        209        349
    ## 36  1.4967320         158          66        536        300
    ## 37  1.2641509         152          29        578        130
    ## 38  1.3211679         186          42        755        169
    ## 39  1.3974359         196          50        768        210
    ## 40  1.9115646         198         126        814        494
    ## 41  1.5299145         234          73        891        194
    ## 42         NA          NA          NA         NA         NA
    ## 43  1.5600000         288          59       1564        143
    ## 44  1.6019108         305         122       2099        554
    ## 45         NA          NA          NA         NA         NA
    ## 46  3.0265957         375         134       2069        493
    ## 47  1.9408602         387          40       2345        323
    ## 48  2.2258065         322          64       1352        197
    ## 49  1.0378788         363          36       1408        137
    ## 50  1.6162791         369          45       1702        132
    ## 51  1.8320312         369         129       1991        453
    ## 52  2.7756410         400         121       1934        390
    ## 53  2.8358209         428         121       1879        346
    ## 54         NA          NA          NA         NA         NA
    ## 55  1.0375000         289          15       1808         83
    ## 56  1.1200000         358          18       2263         55
    ## 57  1.5520000         409          55       2936        188
    ## 58  1.6479401         411          96       2345        414
    ## 59  2.2835821         359          97       2208        431
    ## 60  2.7051282         400          73       2271        189
    ## 61         NA          NA          NA         NA         NA
    ## 62         NA          NA          NA         NA         NA
    ## 63  1.2168675         215          24       1136        101
    ## 64  1.0630631         185          50        703        117
    ## 65  1.5234375         227          56        878        193
    ## 66  2.1348315         272          88       1387        354
    ## 67  2.5482456         276         148       1300        513
    ## 68  2.7571429         260         168       1138        666
    ## 69  1.0877193         235          17       1262         62
    ## 70  1.3647799         297          72       1551        203
    ## 71  3.1857708         318         169       1463        733
    ## 72  3.0000000         197         122        632        243
    ## 73  4.3647541         290         222       1467        916
    ## 74  3.5049505         339         201       1498        640
    ## 75  1.5740741         329          69       2139        249
    ## 76  1.4761905         411         119       2260        393
    ## 77  2.0798817         435         139       2571        630
    ## 78  2.7982833         463         203       2361        538
    ## 79  3.2301136         511         247       2668        932
    ## 80  3.7011952         388         210       1959        864
    ## 81  1.0396040         287           7       1625        105
    ## 82  1.0744681         346          65       1982        297
    ## 83  1.1855072         378          92       2328        368
    ## 84  2.3178295         369         158       1984        557
    ## 85  3.3750000         390         238       1864        755
    ## 86  3.8114035         359         178       1802        793
    ## 87  1.1647059         277          27       1643         99
    ## 88  1.0460526         363          25       1819        159
    ## 89  1.4408602         409          92       1743        252
    ## 90  2.5613208         357         161       1517        495
    ## 91  2.4691358         356          69       1426        160
    ## 92  2.8690476         327         156       1395        410
    ## 93  1.5988372         375          90       2585        254
    ## 94  2.7441077         391         196       2303        733
    ## 95  2.8076923         408         200       2675        825
    ## 96  2.3225806         508         231       3077        793
    ## 97  3.1095238         517         219       2762        583
    ## 98  2.9482072         491         250       2264        710
    ## 99  1.0277778         281           9       1418         37
    ## 100 1.0125000         243           7        999         82
    ## 101 1.0000000         252           2       1145         58
    ## 102        NA          NA          NA         NA         NA
    ## 103 1.0000000         475           6       2088          9
    ## 104 1.0000000         281           3       1454         37
    ## 105 1.7948718         467         108       2555        406
    ## 106 2.7220339         461         160       2687        738
    ## 107 3.3400000         487         201       2479        940
    ## 108 3.2128205         565         207       2965       1092
    ## 109 3.0902778         575         219       2881        769
    ## 110 2.9095355         516         235       2576       1079
    ## 111 1.3595506         283          89       1019        227
    ## 112 1.4867257         315          86       1316        335
    ## 113 1.9052632         298          88       1159        330
    ## 114 1.6179402         248          87        855        455
    ## 115 1.5840000         328          45       1142        197
    ## 116 2.1450382         256          55        995        274
    ## 117 0.1857143         321          16       1787        214
    ## 118 1.0800000         288          73       1822        362
    ## 119 1.6109325         361         132       2092        503
    ## 120 2.2485380         422         153       2485        717
    ## 121 2.2506739         506         219       2504        792
    ## 122 2.9027778         433         155       2389        590
    ## 123 1.0000000         485           8       2826        122
    ## 124 1.0400000         426           9       2524         78
    ## 125 2.0000000         434          13       2764         96
    ## 126 0.2727273         427          10       2283         93
    ## 127 0.2000000         373           2       1963          5
    ## 128 0.5000000         388           2       2077          2
    ## 129        NA          NA          NA         NA         NA
    ## 130        NA          NA          NA         NA         NA
    ## 131        NA          NA          NA         NA         NA
    ## 132 1.2371134         343          36       1698        118
    ## 133 1.7804878         392         121       1843        414
    ## 134 1.4549550         432         112       1811        305
    ## 135 2.3740741         480         169       2256        592
    ## 136 3.7000000         436         178       1965        686
    ## 137 3.0911950         478         221       2044        847
    ## 138 1.4324324         328          41       2138        103
    ## 139 1.4273504         301          61       1990        157
    ## 140 1.0863309         259          29       1332        148
    ## 141 1.0974026         270          21       1054        162
    ## 142 1.4133333         306          50       1518        197
    ## 143 1.3052632         372          47       1748        236
    ## 144 1.0833333         193           6        654         26
    ## 145 1.0000000         434           6       1995         32
    ## 146 1.0000000         344           1       1393         40
    ## 147 1.2032520         353          29       1482        143
    ## 148 1.0000000         303           4       1278         34
    ## 149 1.1680000         309          62       1349        143
    ## 150 1.5827338         373          71       2740        433
    ## 151 2.4847328         419         145       2562        623
    ## 152 2.8042169         455         217       2589        826
    ## 153 3.7310924         553         291       2978       1225
    ## 154 2.7413793         578         298       2940       1145
    ## 155 3.0614525         555         237       2895       1010
    ## 156 3.4000000         278         119       1450        483
    ## 157        NA          NA          NA         NA         NA
    ## 158 3.9196891         333         307       1668       1293
    ## 159 3.5238095         398         188       2518        714
    ## 160 3.2919897         437         261       2410       1154
    ## 161 3.3643411         452         273       3076       1249
    ## 162 1.3661972         317          37       1361         95
    ## 163 1.4228571         287          72       1663        236
    ## 164 1.4976077         318          89       1706        299
    ## 165 1.7405858         364         114       1754        384
    ## 166 2.1791908         327         114       1385        318
    ## 167 2.4800000         385         154       1788        530
    ## 168 1.2758621         178          34        584        111
    ## 169 1.6557377         255          71       1139        258
    ## 170 1.8369099         233         131       1006        403
    ## 171 2.3500000         309         139       1470        502
    ## 172 3.5852535         272         177       1098        622
    ## 173 2.9607843         391         164       1889        521
    ## 174 1.0086207         295           6       1643        117
    ## 175 1.0114286         331          18       2448        177
    ## 176 0.6842105         345          13       2417         39
    ## 177 0.8947368         361          13       2214         39
    ## 178 0.2553191         415           9       2560        179
    ## 179 1.2883436         454          52       2391        204
    ## 180 0.9000000         366          11       2054         21
    ## 181 1.0265487         403          15       2211        116
    ## 182 1.0066667         430          21       2278        151
    ## 183 1.1250000         409          26       1863         79
    ## 184 0.4130435         465          42       2360        193
    ## 185 1.0843373         425         101       2219        195
    ## 186 1.5333333         322          91       1870        319
    ## 187 2.2481481         405         141       2219        542
    ## 188 2.9875260         396         233       1872       1246
    ## 189 2.7272727         480         186       2233        750
    ## 190 2.1420613         491         211       2729        690
    ## 191 2.0717489         475         185       2363        418
    ## 192 1.5000000         228           3        927          3
    ## 193 1.0000000         454           2       1856          7
    ## 194 1.0000000         323           1       1590          1
    ## 195 1.0000000         352           6       2054         14
    ## 196 1.0000000         355           3       1388         33
    ## 197 2.6315789         585          12       2202        100
    ## 198 1.2500000         206          13        788         35
    ## 199 1.0000000         285           2       1393         10
    ## 200 1.0833333         272           3       1504         26
    ## 201 1.2100840         281          18       1726        142
    ## 202 0.6756757         383           9       1917         42
    ## 203 0.7536232         326          14       1852         79
    ## 204 1.3766234         214          67        893        180
    ## 205 1.9481132         220         100        748        368
    ## 206 1.8056872         318         100       1350        365
    ## 207 2.1962617         321         107       1224        438
    ## 208 2.5758929         298         120       1223        537
    ## 209 2.2745098         342         157       1540        605
    ## 210 1.2641509         291          24       1344        260
    ## 211 1.9342916         300         128       1784        879
    ## 212 2.5710145         366         155       2328        702
    ## 213 1.9743590         361          29       2145         74
    ## 214 2.5541796         436         161       2279        736
    ## 215 2.3815789         411         156       2438        611
    ## 216 1.2307692         281           8       1631         16
    ## 217 1.3103448         321          44       1843        107
    ## 218 1.7500000         323          76       1504        218
    ## 219 2.2594142         295         136       1715        477
    ## 220 2.2787611         366         145       1979        459
    ## 221 2.5614754         324         165       1978        568
    ## 222 0.4805825         381          39       1988        337
    ## 223 1.3432836         416         100       2340        433
    ## 224 1.8812665         330         175       1570        694
    ## 225 2.5159817         410         148       2252        510
    ## 226 2.7468750         407         228       2314        815
    ## 227 3.0774194         429         217       2510        897
    ## 228 1.3322785         342          96       2035        398
    ## 229 2.1727273         435         142       2290        439
    ## 230 2.8690476         420         175       2146        825
    ## 231 2.6475410         513         195       2482        576
    ## 232 3.5185185         447         210       1999        800
    ## 233        NA          NA          NA         NA         NA
    ## 234 1.2043011         274          36       1537        109
    ## 235 1.2571429         277          55       1481        126
    ## 236 1.8880000         327          51       1709        232
    ## 237 1.7000000         302          36       1675        135
    ## 238 1.6511628         321          90       1951        340
    ## 239 1.6447368         330          55       1987        110
    ## 240 1.0175439         252           9       1827         58
    ## 241 1.2195122         295          52       1882        148
    ## 242 2.0704225         346         134       2148        404
    ## 243 2.9900000         430         180       2377        493
    ## 244        NA          NA          NA         NA         NA
    ## 245 2.1586207         417         170       2508        571
    ## 246 1.0262009         333          32       1547        235
    ## 247 1.1974249         404          97       1847        264
    ## 248 1.9086294         304         127       1426        352
    ## 249 2.2325581         366         151       1689        434
    ## 250 2.2892308         415         187       1936        651
    ## 251 2.8526316         397         213       1741        702
    ## 252 1.3034483         373          47       2334        176
    ## 253 2.0046729         368         124       2201        423
    ## 254 2.4911032         367         155       2124        604
    ## 255 3.8300395         339         193       1726        820
    ## 256 3.7749077         358         213       1600        875
    ## 257 3.0727969         357         219       1764        719
    ## 258 1.9144981         271         101       1591        450
    ## 259 1.9832215         309         170       2031        536
    ## 260 2.3053892         331         221       2141       1023
    ## 261 2.6837838         390         243       2158        822
    ## 262 2.8665049         396         262       2326       1054
    ## 263 2.6794872         374         219       2227        921
    ## 264 1.2592593         275           9       1417         68
    ## 265 1.3043478         271          52       1515        120
    ## 266 2.2313433         300         102       1740        528
    ## 267 2.5370370         302         122       1556        378
    ## 268 3.2000000         307         131       1207        433
    ## 269        NA          NA          NA         NA         NA
    ## 270 0.5584416         258          20       1188         91
    ## 271 0.9051724         288          41       1375        165
    ## 272 1.1722689         370          95       1912        354
    ## 273 1.7600000         424         108       1909        433
    ## 274 2.2558140         418         142       1791        502
    ## 275        NA          NA          NA         NA         NA
    ## 276        NA          NA          NA         NA         NA
    ## 277 1.1666667         387          10       2144         63
    ## 278 1.2258065         392           6       1973         38
    ## 279 1.0000000         245           2       1018          3
    ## 280 1.1200000         203          11        861         55
    ## 281 1.0000000         344           1       1609         32
    ## 282 1.1610169         351          10       1918        137
    ## 283        NA          NA          NA         NA         NA
    ## 284        NA          NA          NA         NA         NA
    ## 285 1.0761421         260          13       1347        212
    ## 286 2.2481481         295         128       1653        571
    ## 287 2.2893617         364         149       1568        486
    ## 288 3.1622419         343         211       1559        973
    ## 289 2.7388060         315         131       1224        304
    ## 290 2.7600000         322          34       1371         61
    ## 291 1.2500000         303          17       2147         40
    ## 292 1.0000000         272           2       1344          4
    ## 293 1.0000000         280           1       1482         45
    ## 294 1.4102564         369          79       1871        255
    ## 295 1.4734513         319          98       1565        306
    ## 296 1.4258373         255          73       1452        286
    ## 297 2.4967742         408         126       1867        356
    ## 298 3.1124498         323         151       1609        671
    ## 299 3.4809524         340         229       1452        684
    ## 300 3.5371429         425         209       1525        586
    ## 301 3.5950000         383         217       1701        646
    ## 302 1.1000000         265           8       1215         43
    ## 303 1.3628319         381          20       1851        147
    ## 304 1.1666667         368          27       1423         77
    ## 305 1.0000000         358           1       1145         70
    ## 306 0.0000000         364           0       1447          0
    ## 307 1.3333333         370           4       1396          8
    ## 308 1.1818182         331           7       1503         38
    ## 309 1.1976744         309          20       1483         97
    ## 310 1.5529412         333          56       1562        122
    ## 311 3.0289855         363         112       1829        348
    ## 312 2.9215686         388         108       1986        238
    ## 313 2.7612903         383         140       1866        395
    ## 314 1.4086957         441          92       2267        319
    ## 315 1.7359551         415         122       1845        298
    ## 316 3.3037543         474         200       2142        897
    ## 317 3.0775510         554         177       2585        642
    ## 318 2.8321678         563         219       2772        723
    ## 319 2.4292929         548         163       2887        436
    ## 320 1.9397163         236         120       1170        473
    ## 321 3.2179487         197         126        686        449
    ## 322 3.1313559         219         152       1020        660
    ## 323 3.6340694         232         217        798        978
    ## 324 3.8225806         278         217       1067        814
    ## 325 3.2439024         249         102       1024        358
    ## 326 1.3139535         214          32       1118        109
    ## 327 2.0466667         352          88       1905        287
    ## 328 2.3309353         299          85       1418        261
    ## 329 3.4065041         332         153       1699        694
    ## 330 3.6072874         394         244       1977        725
    ## 331 2.8921569         437         183       2347        517
    ## 332 1.1846154         178          11       1144        154
    ## 333 1.7902098         219          70       1058        244
    ## 334 2.4932432         292         168       1470        672
    ## 335 2.8620690         318         205       1524       1051
    ## 336        NA          NA          NA         NA         NA
    ## 337 2.9092742         367         260       1731       1294
    ## 338 1.0444444         195           9        955         47
    ## 339 1.0000000         285           1       1417         19
    ## 340 1.7500000         252           9       1393         14
    ## 341 1.0588235         284           6       1390         36
    ## 342 0.0000000         386           0       2613          0
    ## 343 1.0000000         389           1       2274         44
    ## 344 0.1621622         542           3       2717         41
    ## 345 0.0000000         400           1       2196          1
    ## 346 0.2000000         435           3       2070         30
    ## 347 0.0156250         444           2       2450         64
    ## 348 1.1809045         338          98       2084        233
    ## 349 1.4432990         260          78       1511        260
    ## 350 1.8000000         346         153       1891        393
    ## 351 2.0127796         359         144       2109        574
    ## 352 1.7591463         360         158       2276        531
    ## 353 2.1553398         335         197       2221        618
    ## 354 1.1830065         345          62       1805        244
    ## 355 1.7709251         334         120       1959        401
    ## 356 2.1157895         257         101       1610        424
    ## 357 2.7148289         330         157       1974        637
    ## 358 2.6038462         341         132       1916        636
    ## 359 2.8480000         303         158       1460        659
    ## 360 2.8763441         400         149       2587        469
    ## 361 2.7840000         413         149       2534        670
    ## 362 4.1318681         459         196       2841        698
    ## 363 3.3598326         539         214       3163        693
    ## 364 2.9655172         521         145       3090        357
    ## 365 3.4415584         505         226       3072        713
    ## 366 1.0375000         303          15       1579        166
    ## 367 1.0990991         250          48       1205        356
    ## 368 1.5828571         313          70       1618        254
    ## 369 1.3204225         348          61       1644        334
    ## 370 1.6688963         320         122       1527        511
    ## 371 1.4012346         311          58       1481        210
    ## 372        NA          NA          NA         NA         NA
    ##            Ethnicity Diagnosis Gender   Age severity Socialization
    ## 1              White        TD      M 19.80        0           108
    ## 2              White        TD      M 23.93       NA           110
    ## 3              White        TD      M 27.70       NA           109
    ## 4              White        TD      M 32.90       NA           102
    ## 5              White        TD      M 35.90        0           107
    ## 6              White        TD      M 40.13       NA           100
    ## 7              White       ASD      M 28.80       13            85
    ## 8              White       ASD      M 33.17       NA           105
    ## 9              White       ASD      M 37.07       NA            77
    ## 10             White       ASD      M 41.07       NA            75
    ## 11             White       ASD      M 45.23       10            79
    ## 12             White       ASD      M 49.70       NA            81
    ## 13             White        TD      F 23.50        1            88
    ## 14             White        TD      F 27.83       NA            89
    ## 15             White        TD      F 31.83       NA            91
    ## 16             White        TD      F 35.53       NA            93
    ## 17             White        TD      F 39.47        0            86
    ## 18             White        TD      F 45.07       NA            86
    ## 19      White/Latino       ASD      M 31.03        8            82
    ## 20      White/Latino       ASD      M 35.30       NA           115
    ## 21      White/Latino       ASD      M 38.90       NA            81
    ## 22      White/Latino       ASD      M 43.13       NA            77
    ## 23      White/Latino       ASD      M 47.40        9            79
    ## 24      White/Latino       ASD      M 51.37       NA            74
    ## 25             White       ASD      M 34.03        9            82
    ## 26             White       ASD      M 37.57       NA            94
    ## 27             White       ASD      M 41.57       NA            77
    ## 28             White       ASD      M 45.53       NA            77
    ## 29             White       ASD      M 49.30        9            75
    ## 30             White       ASD      M 54.13       NA            75
    ## 31       Bangladeshi       ASD      F 26.17       17            68
    ## 32       Bangladeshi       ASD      F 30.27       NA            74
    ## 33       Bangladeshi       ASD      F 34.57       NA            70
    ## 34       Bangladeshi       ASD      F 38.47       NA            66
    ## 35       Bangladeshi       ASD      F 42.43        8            68
    ## 36       Bangladeshi       ASD      F 46.53       NA            74
    ## 37             White       ASD      F 41.00       18            65
    ## 38             White       ASD      F 49.20       NA            63
    ## 39             White       ASD      F 49.20       NA            63
    ## 40             White       ASD      F 52.27       NA            61
    ## 41             White       ASD      F 57.73       17            63
    ## 42             White       ASD      F 60.33       NA            59
    ## 43             White        TD      M 20.10        5           102
    ## 44             White        TD      M 24.07       NA           102
    ## 45             White        TD      M 28.17       NA           109
    ## 46             White        TD      M 32.07       NA           109
    ## 47             White        TD      M 36.07        5           100
    ## 48             White        TD      M 40.17       NA           108
    ## 49             White        TD      F 18.30        0           100
    ## 50             White        TD      F 23.73       NA           106
    ## 51             White        TD      F 26.63       NA           105
    ## 52             White        TD      F 31.07       NA           103
    ## 53             White        TD      F 35.00        1           107
    ## 54             White        TD      F    NA       NA            NA
    ## 55             White        TD      M 19.27        3           104
    ## 56             White        TD      M 23.83       NA           106
    ## 57             White        TD      M 28.53       NA           111
    ## 58             White        TD      M 32.43       NA           103
    ## 59             White        TD      M 36.53        5           103
    ## 60             White        TD      M 40.43       NA           103
    ## 61             White       ASD      F 31.77        0           105
    ## 62    White American       ASD      F 37.13       NA           110
    ## 63             White        TD      M 19.23        0           106
    ## 64             White        TD      M    NA       NA           104
    ## 65             White        TD      M 27.27       NA           102
    ## 66             White        TD      M 31.20       NA           100
    ## 67             White        TD      M 35.63        0           100
    ## 68             White        TD      M 40.27       NA           101
    ## 69             White        TD      M 20.07        0           104
    ## 70             White        TD      M 23.83       NA           115
    ## 71             White        TD      M 28.27       NA           105
    ## 72             White        TD      M 32.07       NA           103
    ## 73             White        TD      M 35.87        0           101
    ## 74             White        TD      M 41.50       NA            97
    ## 75             White        TD      M 19.00        0           102
    ## 76             White        TD      M 22.97       NA           100
    ## 77             White        TD      M 27.07       NA           109
    ## 78             White        TD      M 31.03       NA           107
    ## 79             White        TD      M 35.37        0           100
    ## 80             White        TD      M 39.40       NA            92
    ## 81             White        TD      M 18.97        0            92
    ## 82             White        TD      M 22.87       NA           102
    ## 83             White        TD      M 26.80       NA            91
    ## 84             White        TD      M 31.47       NA            96
    ## 85             White        TD      M 35.10        0            87
    ## 86             White        TD      M 39.43       NA            83
    ## 87             White        TD      M 19.27        0            86
    ## 88             White        TD      M 24.80       NA            86
    ## 89             White        TD      M 28.10       NA            86
    ## 90             White        TD      M 31.27       NA            87
    ## 91             White        TD      M 35.90        3            86
    ## 92             White        TD      M 40.30       NA            94
    ## 93             White        TD      M 21.67        0           102
    ## 94             White        TD      M 26.27       NA           102
    ## 95             White        TD      M 30.63       NA            95
    ## 96             White        TD      M 34.27       NA           100
    ## 97             White        TD      M 38.17        0           103
    ## 98             White        TD      M 42.93       NA            97
    ## 99             White       ASD      M 34.80       14            74
    ## 100            White       ASD      M 38.73       NA           104
    ## 101            White       ASD      M    NA       NA            68
    ## 102            White       ASD      M 46.67       NA            68
    ## 103            White       ASD      M 50.93       15            66
    ## 104            White       ASD      M 54.43       NA            65
    ## 105            White        TD      M 20.77        0           106
    ## 106            White        TD      M 26.13       NA           107
    ## 107            White        TD      M 30.03       NA           112
    ## 108            White        TD      M 34.43       NA           105
    ## 109            White        TD      M 38.70        0           118
    ## 110            White        TD      M 44.07       NA           116
    ## 111            White       ASD      M 35.80       11            88
    ## 112            White       ASD      M 39.93       NA           102
    ## 113            White       ASD      M 43.90       NA            79
    ## 114            White       ASD      M 47.83       NA            74
    ## 115            White       ASD      M 51.97        9            72
    ## 116            White       ASD      M 56.73       NA            72
    ## 117            White       ASD      M 18.77        9            86
    ## 118            White       ASD      M 22.50       NA            84
    ## 119            White       ASD      M 26.77       NA            95
    ## 120            White       ASD      M 30.80       NA            98
    ## 121            White       ASD      M 34.13        8           107
    ## 122            White       ASD      M 37.30       NA           103
    ## 123 African American       ASD      M 27.53       21            72
    ## 124 African American       ASD      M 31.80       NA            74
    ## 125 African American       ASD      M 36.17       NA            77
    ## 126 African American       ASD      M 40.27       NA            68
    ## 127 African American       ASD      M 44.70       19            72
    ## 128 African American       ASD      M 48.97       NA            65
    ## 129           Latino       ASD      M 35.47       19            78
    ## 130         Hispanic       ASD      M 39.87       NA            70
    ## 131  Latino/Hispanic       ASD      M 43.57       NA            81
    ## 132            White        TD      F 18.93        0           106
    ## 133            White        TD      F 22.90       NA           102
    ## 134            White        TD      F 26.67       NA           114
    ## 135            White        TD      F 30.93       NA           109
    ## 136            White        TD      F 35.13        0           114
    ## 137            White        TD      F 39.23       NA           110
    ## 138     White/Latino       ASD      M 27.37       14            65
    ## 139     White/Latino       ASD      M 32.10       NA            65
    ## 140     White/Latino       ASD      M 35.93       NA            70
    ## 141     White/Latino       ASD      M 42.23       NA            63
    ## 142     White/Latino       ASD      M 44.93       14            65
    ## 143     White/Latino       ASD      M 47.50       NA            65
    ## 144            White       ASD      M 37.47       20            67
    ## 145            White       ASD      M 42.20       NA            78
    ## 146            White       ASD      M 46.37       NA            74
    ## 147            White       ASD      M 52.13       NA            72
    ## 148            White       ASD      M 57.20       17            72
    ## 149            White       ASD      M 62.40       NA            68
    ## 150            White        TD      M 21.77        0            90
    ## 151            White        TD      M 25.47       NA           108
    ## 152            White        TD      M 30.13       NA           100
    ## 153            White        TD      M 34.00       NA           102
    ## 154            White        TD      M 37.93        0           101
    ## 155            White        TD      M 42.47       NA           103
    ## 156            White       ASD      M 30.40       11           100
    ## 157            White       ASD      M 34.43       NA            70
    ## 158            White       ASD      M 38.60       NA            83
    ## 159            White       ASD      M 42.63       NA            86
    ## 160            White       ASD      M 46.93        4            97
    ## 161            White       ASD      M 51.00       NA            90
    ## 162            White        TD      M 21.03        0            96
    ## 163            White        TD      M 25.23       NA            98
    ## 164            White        TD      M 29.07       NA            98
    ## 165            White        TD      M 33.30       NA           100
    ## 166            White        TD      M 37.73        0           112
    ## 167            White        TD      M 41.17       NA           106
    ## 168            White        TD      M 19.93        0           102
    ## 169            White        TD      M 23.57       NA            76
    ## 170            White        TD      M 27.03       NA           100
    ## 171            White        TD      M 31.00       NA            96
    ## 172            White        TD      M 36.43        0           100
    ## 173            White        TD      M 39.43       NA            95
    ## 174            White       ASD      M 34.87       17            72
    ## 175            White       ASD      M 38.13       NA            68
    ## 176            White       ASD      M 43.10       NA            72
    ## 177            White       ASD      M 46.63       NA            72
    ## 178            White       ASD      M 50.97       20            68
    ## 179            White       ASD      M 55.17       NA            72
    ## 180            White       ASD      M 36.53       12            70
    ## 181            White       ASD      M 40.50       NA            79
    ## 182            White       ASD      M 44.67       NA            79
    ## 183            White       ASD      M 48.10       NA            74
    ## 184            White       ASD      M 52.90       15            77
    ## 185            White       ASD      M 56.43       NA            85
    ## 186            White        TD      M 23.13        0           100
    ## 187            White        TD      M 27.70       NA           103
    ## 188            White        TD      M 32.07       NA           107
    ## 189            White        TD      M 35.03       NA           107
    ## 190            White        TD      M 39.53        0           106
    ## 191            White        TD      M 43.80       NA           108
    ## 192            White       ASD      M 34.27       21            69
    ## 193            White       ASD      M 38.33       NA            80
    ## 194            White       ASD      M 42.13       NA            68
    ## 195            White       ASD      M 47.10       NA            72
    ## 196            White       ASD      M 50.73       20            66
    ## 197            White       ASD      M 54.63       NA            66
    ## 198 African American       ASD      F 25.33       14            76
    ## 199 African American       ASD      F 29.70       NA            74
    ## 200 African American       ASD      F 34.10       NA            74
    ## 201 African American       ASD      F 36.93       NA            86
    ## 202 African American       ASD      F 42.97       25            85
    ## 203 African American       ASD      F 46.17       NA            83
    ## 204            White       ASD      M 33.77       10            82
    ## 205            White       ASD      M 38.13       NA            74
    ## 206            White       ASD      M 43.00       NA            92
    ## 207            White       ASD      M 46.13       NA            92
    ## 208            White       ASD      M 50.30       16            90
    ## 209            White       ASD      M 53.77       NA            92
    ## 210            White        TD      M 19.30        1           102
    ## 211            White        TD      M 23.43       NA           105
    ## 212            White        TD      M 27.13       NA           112
    ## 213            White        TD      M 30.80       NA           105
    ## 214            White        TD      M 34.93        2           103
    ## 215            White        TD      M 39.07       NA            97
    ## 216            White        TD      M 19.20        3            98
    ## 217            White        TD      M 22.87       NA           102
    ## 218            White        TD      M 26.87       NA           103
    ## 219            White        TD      M 30.40       NA           100
    ## 220            White        TD      M 34.40        2            91
    ## 221            White        TD      M 38.53       NA            97
    ## 222            White       ASD      M 39.50        7            70
    ## 223            White       ASD      M 43.68       NA            83
    ## 224            White       ASD      M 47.73       NA           100
    ## 225            White       ASD      M 51.67       NA           101
    ## 226            White       ASD      M 55.17        4           105
    ## 227            White       ASD      M 58.77       NA           116
    ## 228            White        TD      F 19.87        1           104
    ## 229            White        TD      F 24.17       NA           116
    ## 230            White        TD      F 28.07       NA           125
    ## 231            White        TD      F 32.17       NA           125
    ## 232            White        TD      F 36.13        0           120
    ## 233            White        TD      F 40.23       NA           114
    ## 234      White/Asian       ASD      M 33.20       11            88
    ## 235      White/Asian       ASD      M 37.37       NA            72
    ## 236      White/Asian       ASD      M 41.73       NA            63
    ## 237      White/Asian       ASD      M 45.33       NA            66
    ## 238      White/Asian       ASD      M 48.63       11            63
    ## 239      White/Asian       ASD      M 53.63       NA            63
    ## 240         Lebanese       ASD      M 24.90       13            86
    ## 241         Lebanese       ASD      M 32.20       NA            38
    ## 242         Lebanese       ASD      M 34.00       NA            87
    ## 243         Lebanese       ASD      M    NA       NA            86
    ## 244         Lebanese       ASD      M 42.27       12            83
    ## 245         Lebanese       ASD      M 46.40       NA            94
    ## 246            White        TD      M 19.23        0           102
    ## 247            White        TD      M 23.07       NA            92
    ## 248            White        TD      M 28.47       NA           103
    ## 249            White        TD      M 32.07       NA           100
    ## 250            White        TD      M 35.67        0           105
    ## 251            White        TD      M 39.93       NA           101
    ## 252            White        TD      M 19.37        0            96
    ## 253            White        TD      M    NA       NA           105
    ## 254            White        TD      M 28.90       NA           109
    ## 255            White        TD      M 32.07       NA           109
    ## 256            White        TD      M 36.40        0           100
    ## 257            White        TD      M 40.13       NA           101
    ## 258            White       ASD      M 37.03       14            65
    ## 259            White       ASD      M 42.77       NA            72
    ## 260            White       ASD      M 46.23       NA            70
    ## 261            White       ASD      M 49.30       NA            75
    ## 262            White       ASD      M 53.40       11            77
    ## 263            White       ASD      M 57.37       NA            77
    ## 264            White        TD      M 19.77        3            94
    ## 265            White        TD      M 23.63       NA           106
    ## 266            White        TD      M 27.67       NA           100
    ## 267            White        TD      M 32.07       NA            95
    ## 268            White        TD      M 35.83        1           107
    ## 269            White        TD      M 39.93       NA           118
    ## 270            White        TD      M 20.03        5           113
    ## 271            White        TD      M 24.68       NA           118
    ## 272            White        TD      M 28.63       NA           112
    ## 273            White        TD      M 32.77       NA           114
    ## 274            White        TD      M 36.70       10           122
    ## 275            White        TD      M 23.13        0            92
    ## 276            White        TD      M    NA       NA            93
    ## 277            White       ASD      M 36.73       20            75
    ## 278            White       ASD      M 41.50       NA            74
    ## 279            White       ASD      M 45.57       NA            61
    ## 280            White       ASD      M 50.23       NA            55
    ## 281            White       ASD      M 54.33       17            63
    ## 282            White       ASD      M 57.43       NA            59
    ## 283            White        TD      M    NA        4            84
    ## 284            White        TD      M    NA       NA            91
    ## 285            White        TD      F 20.03        0           102
    ## 286            White        TD      F 24.40       NA           100
    ## 287            White        TD      F 29.53       NA            98
    ## 288            White        TD      F 32.13       NA            93
    ## 289            White        TD      F 39.10       NA            95
    ## 290            White        TD      F 40.37       NA           101
    ## 291            White       ASD      M 31.63       17            82
    ## 292            White       ASD      M 37.20       NA            66
    ## 293            White       ASD      M 43.63       NA            68
    ## 294            White       ASD      M 52.77       17            68
    ## 295            White       ASD      M    NA       NA            63
    ## 296            White        TD      M 23.07        0           108
    ## 297            White        TD      M 27.47       NA            70
    ## 298            White        TD      M 31.63       NA           105
    ## 299            White        TD      M 35.63       NA           105
    ## 300            White        TD      M 39.47        0           101
    ## 301            White        TD      M 43.40       NA           101
    ## 302            White       ASD      M 37.47       19            64
    ## 303            White       ASD      M 42.20       NA            77
    ## 304            White       ASD      M 46.37       NA            72
    ## 305            White       ASD      M 52.13       NA            68
    ## 306            White       ASD      M 57.20       17            68
    ## 307            White       ASD      M 62.40       NA            66
    ## 308            Asian        TD      F 20.87        1           102
    ## 309            Asian        TD      F 25.40       NA           109
    ## 310            Asian        TD      F 29.67       NA           105
    ## 311            Asian        TD      F 34.43       NA           120
    ## 312            Asian        TD      F 37.67        3           112
    ## 313            Asian        TD      F 42.10       NA           108
    ## 314            White        TD      M 22.57        0            94
    ## 315            White        TD      M 26.63       NA            81
    ## 316            White        TD      M 30.77       NA           103
    ## 317            White        TD      M 35.03       NA           100
    ## 318            White        TD      M 38.60        0            94
    ## 319            White        TD      M 43.03       NA            97
    ## 320            White        TD      M 23.90        0            98
    ## 321            White        TD      M 28.60       NA            86
    ## 322            White        TD      M 32.50       NA           107
    ## 323            White        TD      M 36.40       NA           108
    ## 324            White        TD      M 40.07        0           103
    ## 325            White        TD      M 44.43       NA           101
    ## 326            White        TD      M 19.10        1           115
    ## 327            White        TD      M 23.17       NA            59
    ## 328            White        TD      M 27.07       NA           107
    ## 329            White        TD      M 30.83       NA           105
    ## 330            White        TD      M 35.17        0           105
    ## 331            White        TD      M 39.30       NA           101
    ## 332            White        TD      M 19.97        0            96
    ## 333            White        TD      M 23.93       NA            96
    ## 334            White        TD      M 28.03       NA            91
    ## 335            White        TD      M 32.03       NA            93
    ## 336            White        TD      M 36.83        0           100
    ## 337            White        TD      M 41.93       NA            95
    ## 338            White       ASD      M 35.50       14            77
    ## 339            White       ASD      M 39.40       NA            65
    ## 340            White       ASD      M 44.47       NA            70
    ## 341            White       ASD      M    NA       NA            NA
    ## 342            White       ASD      F 41.07       15            66
    ## 343            White       ASD      F 41.07       NA            66
    ## 344            White       ASD      F 49.97       NA            65
    ## 345            White       ASD      F 53.90       NA            68
    ## 346            White       ASD      F 58.27       13            66
    ## 347            White       ASD      F 61.70       NA            68
    ## 348            White       ASD      M 26.00       15            88
    ## 349            White       ASD      M 30.10       NA            72
    ## 350            White       ASD      M 34.27       NA            86
    ## 351            White       ASD      M 38.20       NA            83
    ## 352            White       ASD      M 42.30       16            83
    ## 353            White       ASD      M 46.07       NA            79
    ## 354            White        TD      M 20.80        4           108
    ## 355            White        TD      M 25.87       NA           118
    ## 356            White        TD      M 29.67       NA           116
    ## 357            White        TD      M 33.60       NA           116
    ## 358            White        TD      M 37.34        5           116
    ## 359            White        TD      M 41.00       NA            NA
    ## 360            White       ASD      M 34.00       13            87
    ## 361            White       ASD      M 38.63       NA            46
    ## 362            White       ASD      M 42.47       NA           100
    ## 363            White       ASD      M 47.00       NA           100
    ## 364            White       ASD      M 51.13       15            92
    ## 365            White       ASD      M 54.73       NA            97
    ## 366            White       ASD      M 42.00       15            79
    ## 367            White       ASD      M 46.77       NA            79
    ## 368            White       ASD      M 50.63       NA            94
    ## 369            White       ASD      M 54.13       NA            92
    ## 370            White       ASD      M 58.57       15            95
    ## 371            White       ASD      M 62.33       NA           103
    ## 372            White        TD      M 18.07       15           104
    ##     nonVerbalIQ verbalIQ    mtoal
    ## 1            28       14 389.8011
    ## 2            NA       NA 389.8011
    ## 3            NA       NA 389.8011
    ## 4            33       NA 389.8011
    ## 5            NA       NA 389.8011
    ## 6            42       44 389.8011
    ## 7            34       27 389.8011
    ## 8            NA       NA 389.8011
    ## 9            NA       NA 389.8011
    ## 10           49       NA 389.8011
    ## 11           NA       NA 389.8011
    ## 12           48       48 389.8011
    ## 13           29       18 389.8011
    ## 14           NA       NA 389.8011
    ## 15           NA       NA 389.8011
    ## 16           39       NA 389.8011
    ## 17           NA       NA 389.8011
    ## 18           45       45 389.8011
    ## 19           31       27 389.8011
    ## 20           NA       NA 389.8011
    ## 21           NA       NA 389.8011
    ## 22           41       NA 389.8011
    ## 23           NA       NA 389.8011
    ## 24           44       44 389.8011
    ## 25           34       27 389.8011
    ## 26           NA       NA 389.8011
    ## 27           NA       NA 389.8011
    ## 28           38       NA 389.8011
    ## 29           NA       NA 389.8011
    ## 30           40       37 389.8011
    ## 31           20       17 389.8011
    ## 32           NA       NA 389.8011
    ## 33           NA       NA 389.8011
    ## 34           33       NA 389.8011
    ## 35           NA       NA 389.8011
    ## 36           39       26 389.8011
    ## 37           24       14 389.8011
    ## 38           NA       NA 389.8011
    ## 39           NA       NA 389.8011
    ## 40           29       NA 389.8011
    ## 41           NA       NA 389.8011
    ## 42           32       27 389.8011
    ## 43           32       31 389.8011
    ## 44           NA       NA 389.8011
    ## 45           NA       NA 389.8011
    ## 46           42       NA 389.8011
    ## 47           NA       NA 389.8011
    ## 48           43       39 389.8011
    ## 49           24       18 389.8011
    ## 50           NA       NA 389.8011
    ## 51           NA       NA 389.8011
    ## 52           45       NA 389.8011
    ## 53           NA       NA 389.8011
    ## 54           NA       NA 389.8011
    ## 55           27       18 389.8011
    ## 56           NA       NA 389.8011
    ## 57           NA       NA 389.8011
    ## 58           31       NA 389.8011
    ## 59           NA       NA 389.8011
    ## 60           33       33 389.8011
    ## 61           28       33 389.8011
    ## 62           NA       NA 389.8011
    ## 63           21       15 389.8011
    ## 64           NA       NA 389.8011
    ## 65           NA       NA 389.8011
    ## 66           35       NA 389.8011
    ## 67           NA       NA 389.8011
    ## 68           42       27 389.8011
    ## 69           30       16 389.8011
    ## 70           NA       NA 389.8011
    ## 71           NA       NA 389.8011
    ## 72           42       NA 389.8011
    ## 73           NA       NA 389.8011
    ## 74           44       47 389.8011
    ## 75           25       17 389.8011
    ## 76           NA       NA 389.8011
    ## 77           NA       NA 389.8011
    ## 78           40       NA 389.8011
    ## 79           NA       NA 389.8011
    ## 80           47       41 389.8011
    ## 81           23       17 389.8011
    ## 82           NA       NA 389.8011
    ## 83           NA       NA 389.8011
    ## 84           29       NA 389.8011
    ## 85           NA       NA 389.8011
    ## 86           44       37 389.8011
    ## 87           24       15 389.8011
    ## 88           NA       NA 389.8011
    ## 89           NA       NA 389.8011
    ## 90           33       NA 389.8011
    ## 91           NA       NA 389.8011
    ## 92           40       40 389.8011
    ## 93           29       26 389.8011
    ## 94           NA       NA 389.8011
    ## 95           NA       NA 389.8011
    ## 96           45       NA 389.8011
    ## 97           NA       NA 389.8011
    ## 98           49       46 389.8011
    ## 99           25       11 389.8011
    ## 100          NA       NA 389.8011
    ## 101          NA       NA 389.8011
    ## 102          27       NA 389.8011
    ## 103          NA       NA 389.8011
    ## 104          27       11 389.8011
    ## 105          29       33 389.8011
    ## 106          NA       NA 389.8011
    ## 107          NA       NA 389.8011
    ## 108          50       NA 389.8011
    ## 109          NA       NA 389.8011
    ## 110          50       50 389.8011
    ## 111          28       20 389.8011
    ## 112          NA       NA 389.8011
    ## 113          NA       NA 389.8011
    ## 114          35       NA 389.8011
    ## 115          NA       NA 389.8011
    ## 116          30       32 389.8011
    ## 117          26       14 389.8011
    ## 118          NA       NA 389.8011
    ## 119          NA       NA 389.8011
    ## 120          43       NA 389.8011
    ## 121          NA       NA 389.8011
    ## 122          43       48 389.8011
    ## 123          22        8 389.8011
    ## 124          NA       NA 389.8011
    ## 125          NA       NA 389.8011
    ## 126          27       NA 389.8011
    ## 127          NA       NA 389.8011
    ## 128          26       16 389.8011
    ## 129          26       16 389.8011
    ## 130          NA       NA 389.8011
    ## 131          NA       NA 389.8011
    ## 132          21       19 389.8011
    ## 133          NA       NA 389.8011
    ## 134          NA       NA 389.8011
    ## 135          40       NA 389.8011
    ## 136          NA       NA 389.8011
    ## 137          43       40 389.8011
    ## 138          25       19 389.8011
    ## 139          NA       NA 389.8011
    ## 140          NA       NA 389.8011
    ## 141          29       NA 389.8011
    ## 142          NA       NA 389.8011
    ## 143          30       18 389.8011
    ## 144          13       11 389.8011
    ## 145          NA       NA 389.8011
    ## 146          NA       NA 389.8011
    ## 147          29       NA 389.8011
    ## 148          NA       NA 389.8011
    ## 149          30       12 389.8011
    ## 150          29       22 389.8011
    ## 151          NA       NA 389.8011
    ## 152          NA       NA 389.8011
    ## 153          47       NA 389.8011
    ## 154          NA       NA 389.8011
    ## 155          48       39 389.8011
    ## 156          32       33 389.8011
    ## 157          NA       NA 389.8011
    ## 158          NA       NA 389.8011
    ## 159          41       NA 389.8011
    ## 160          NA       NA 389.8011
    ## 161          46       46 389.8011
    ## 162          26       18 389.8011
    ## 163          NA       NA 389.8011
    ## 164          NA       NA 389.8011
    ## 165          42       NA 389.8011
    ## 166          NA       NA 389.8011
    ## 167          43       35 389.8011
    ## 168          20       16 389.8011
    ## 169          NA       NA 389.8011
    ## 170          NA       NA 389.8011
    ## 171          28       NA 389.8011
    ## 172          NA       NA 389.8011
    ## 173          39       32 389.8011
    ## 174          26       14 389.8011
    ## 175          NA       NA 389.8011
    ## 176          NA       NA 389.8011
    ## 177          27       NA 389.8011
    ## 178          NA       NA 389.8011
    ## 179          39       21 389.8011
    ## 180          31       13 389.8011
    ## 181          NA       NA 389.8011
    ## 182          NA       NA 389.8011
    ## 183          47       NA 389.8011
    ## 184          NA       NA 389.8011
    ## 185          49       22 389.8011
    ## 186          27       22 389.8011
    ## 187          NA       NA 389.8011
    ## 188          NA       NA 389.8011
    ## 189          43       NA 389.8011
    ## 190          NA       NA 389.8011
    ## 191          45       45 389.8011
    ## 192          21        9 389.8011
    ## 193          NA       NA 389.8011
    ## 194          NA       NA 389.8011
    ## 195          27       NA 389.8011
    ## 196          NA       NA 389.8011
    ## 197          28       10 389.8011
    ## 198          25       11 389.8011
    ## 199          NA       NA 389.8011
    ## 200          NA       NA 389.8011
    ## 201          36       NA 389.8011
    ## 202          NA       NA 389.8011
    ## 203          33       18 389.8011
    ## 204          27       22 389.8011
    ## 205          NA       NA 389.8011
    ## 206          NA       NA 389.8011
    ## 207          31       NA 389.8011
    ## 208          NA       NA 389.8011
    ## 209          44       32 389.8011
    ## 210          23       21 389.8011
    ## 211          NA       NA 389.8011
    ## 212          NA       NA 389.8011
    ## 213          36       NA 389.8011
    ## 214          NA       NA 389.8011
    ## 215          45       34 389.8011
    ## 216          19       13 389.8011
    ## 217          NA       NA 389.8011
    ## 218          NA       NA 389.8011
    ## 219          35       NA 389.8011
    ## 220          NA       NA 389.8011
    ## 221          46       31 389.8011
    ## 222          33       26 389.8011
    ## 223          NA       NA 389.8011
    ## 224          NA       NA 389.8011
    ## 225          49       NA 389.8011
    ## 226          NA       NA 389.8011
    ## 227          50       48 389.8011
    ## 228          29       28 389.8011
    ## 229          NA       NA 389.8011
    ## 230          NA       NA 389.8011
    ## 231          44       NA 389.8011
    ## 232          NA       NA 389.8011
    ## 233          49       43 389.8011
    ## 234          26       19 389.8011
    ## 235          NA       NA 389.8011
    ## 236          NA       NA 389.8011
    ## 237          45       NA 389.8011
    ## 238          NA       NA 389.8011
    ## 239          30       29 389.8011
    ## 240          27       13 389.8011
    ## 241          NA       NA 389.8011
    ## 242          NA       NA 389.8011
    ## 243          NA       NA 389.8011
    ## 244          NA       NA 389.8011
    ## 245          41       37 389.8011
    ## 246          25       17 389.8011
    ## 247          NA       NA 389.8011
    ## 248          NA       NA 389.8011
    ## 249          41       NA 389.8011
    ## 250          NA       NA 389.8011
    ## 251          45       36 389.8011
    ## 252          24       19 389.8011
    ## 253          NA       NA 389.8011
    ## 254          NA       NA 389.8011
    ## 255          39       NA 389.8011
    ## 256          NA       NA 389.8011
    ## 257          46       46 389.8011
    ## 258          42       27 389.8011
    ## 259          NA       NA 389.8011
    ## 260          NA       NA 389.8011
    ## 261          50       NA 389.8011
    ## 262          NA       NA 389.8011
    ## 263          49       45 389.8011
    ## 264          30       20 389.8011
    ## 265          NA       NA 389.8011
    ## 266          NA       NA 389.8011
    ## 267          40       NA 389.8011
    ## 268          NA       NA 389.8011
    ## 269          40       42 389.8011
    ## 270          24       20 389.8011
    ## 271          NA       NA 389.8011
    ## 272          NA       NA 389.8011
    ## 273          26       NA 389.8011
    ## 274          NA       NA 389.8011
    ## 275          27       17 389.8011
    ## 276          NA       NA 389.8011
    ## 277          21        9 389.8011
    ## 278          NA       NA 389.8011
    ## 279          NA       NA 389.8011
    ## 280          29       NA 389.8011
    ## 281          NA       NA 389.8011
    ## 282          32       10 389.8011
    ## 283          17       16 389.8011
    ## 284          NA       NA 389.8011
    ## 285          27       20 389.8011
    ## 286          NA       NA 389.8011
    ## 287          NA       NA 389.8011
    ## 288          42       NA 389.8011
    ## 289          NA       NA 389.8011
    ## 290          43       46 389.8011
    ## 291          28       10 389.8011
    ## 292          NA       NA 389.8011
    ## 293          33       NA 389.8011
    ## 294          NA       NA 389.8011
    ## 295          32       21 389.8011
    ## 296          27       27 389.8011
    ## 297          NA       NA 389.8011
    ## 298          NA       NA 389.8011
    ## 299          45       NA 389.8011
    ## 300          NA       NA 389.8011
    ## 301          45       48 389.8011
    ## 302          17       10 389.8011
    ## 303          NA       NA 389.8011
    ## 304          NA       NA 389.8011
    ## 305          22       NA 389.8011
    ## 306          NA       NA 389.8011
    ## 307          24       11 389.8011
    ## 308          22       14 389.8011
    ## 309          NA       NA 389.8011
    ## 310          NA       NA 389.8011
    ## 311          30       NA 389.8011
    ## 312          NA       NA 389.8011
    ## 313          46       40 389.8011
    ## 314          29       22 389.8011
    ## 315          NA       NA 389.8011
    ## 316          NA       NA 389.8011
    ## 317          45       NA 389.8011
    ## 318          NA       NA 389.8011
    ## 319          47       44 389.8011
    ## 320          30       30 389.8011
    ## 321          NA       NA 389.8011
    ## 322          NA       NA 389.8011
    ## 323          45       NA 389.8011
    ## 324          NA       NA 389.8011
    ## 325          45       36 389.8011
    ## 326          24       22 389.8011
    ## 327          NA       NA 389.8011
    ## 328          NA       NA 389.8011
    ## 329          39       NA 389.8011
    ## 330          NA       NA 389.8011
    ## 331          41       41 389.8011
    ## 332          26       17 389.8011
    ## 333          NA       NA 389.8011
    ## 334          NA       NA 389.8011
    ## 335          40       NA 389.8011
    ## 336          NA       NA 389.8011
    ## 337          45       38 389.8011
    ## 338          27       11 389.8011
    ## 339          NA       NA 389.8011
    ## 340          NA       NA 389.8011
    ## 341          34       17 389.8011
    ## 342          28       10 389.8011
    ## 343          NA       NA 389.8011
    ## 344          NA       NA 389.8011
    ## 345          30       NA 389.8011
    ## 346          NA       NA 389.8011
    ## 347          32        9 389.8011
    ## 348          30       24 389.8011
    ## 349          NA       NA 389.8011
    ## 350          NA       NA 389.8011
    ## 351          40       NA 389.8011
    ## 352          NA       NA 389.8011
    ## 353          45       30 389.8011
    ## 354          29       22 389.8011
    ## 355          NA       NA 389.8011
    ## 356          NA       NA 389.8011
    ## 357          34       NA 389.8011
    ## 358          NA       NA 389.8011
    ## 359          NA       NA 389.8011
    ## 360          30       30 389.8011
    ## 361          NA       NA 389.8011
    ## 362          NA       NA 389.8011
    ## 363          47       NA 389.8011
    ## 364          NA       NA 389.8011
    ## 365          50       50 389.8011
    ## 366          27       16 389.8011
    ## 367          NA       NA 389.8011
    ## 368          NA       NA 389.8011
    ## 369          28       NA 389.8011
    ## 370          NA       NA 389.8011
    ## 371          41       31 389.8011
    ## 372          NA       NA 389.8011

``` r
# 2
df %>% 
  group_by(SUBJ) %>% 
  mutate(mtoal = mean(totalW_CHI, na.rm = T))
```

    ## # A tibble: 372 x 21
    ## # Groups:   SUBJ [66]
    ##     SUBJ severity1 Socialization1 nonVerbalIQ1 verbalIQ1 VISIT MOT_MLU
    ##    <dbl>     <int>          <int>        <int>     <int> <dbl>   <dbl>
    ##  1     1         0            108           28        14     1    3.62
    ##  2     1         0            108           28        14     2    3.86
    ##  3     1         0            108           28        14     3    4.32
    ##  4     1         0            108           28        14     4    4.42
    ##  5     1         0            108           28        14     5    5.21
    ##  6     1         0            108           28        14     6    4.66
    ##  7     2        13             85           34        27     1    4.10
    ##  8     2        13             85           34        27     2    4.96
    ##  9     2        13             85           34        27     3    4.15
    ## 10     2        13             85           34        27     4    5.31
    ## # … with 362 more rows, and 14 more variables: CHI_MLU <dbl>,
    ## #   uniqueW_MOT <int>, uniqueW_CHI <int>, totalW_MOT <int>,
    ## #   totalW_CHI <int>, Ethnicity <fct>, Diagnosis <chr>, Gender <chr>,
    ## #   Age <dbl>, severity <int>, Socialization <int>, nonVerbalIQ <int>,
    ## #   verbalIQ <int>, mtoal <dbl>

``` r
# 3
# the mean doesnt tell us much about the development of the child
```
