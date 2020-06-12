\cleardoublepage

\appendix




# Fisheries Statistics {#appendixA}

## Data Loading  and Cleaning

Screening and cleaning data was done first to identify and fix any potential errors (missing data, typos, errors, etc.). This was the first thing we did before  we analyzed the data. Because the data were gathered from two different sources, screening and cleaning them by sorting and filtering the data columns and structure them for common variables. 

## Loading Data

Throughout the processes of analysing fisheries data and plotting make plots, we’ll use a common data set. First, we need to create a create a new folder in our working directory called `_data`. Then, we copied the files gathered from CAS and e--CAS into `_data` folder. Then we use `read_csv()` function from **readr** package [@readr], to import the sheet in the Excel spreadsheet into data frame in R [@R-base]. Let’s read this dataset into R and determine the structure of the dataset. The landings_data data frame is from a fishery-dependent landing site survey and e--CAS. The dataset has seven variables, which are;

+ Year: the year the data was recorded
+ Family: the family the fish species belong
+ Eng_name: Scientific name of the species
+ Swahil-name: the local name of the species in *swahili* language
+ Districts: the district name the fish catch was landed
+ Catch: the fish catch landed
+ Zone: whether the fish was landed in RUMAKI or Non-RUMAKI fishing areas.


```r
fisheries.data = readr::read_csv("../data_update/All data combined_cleaned.csv")
```


We can look on the internal structure of the dataset with `glimpse` function from **dplyr** package. We notice that except the Year, which is numerical, all the other variables are string as character. Surprising the catch data was entered into the dataset as character instead of numerical---decimal places. The Year also need to be converted to integer. Appendix \@ref(tab:tab0) show the sampled observation of the fisheries dataset. 


```r
fisheries.data %>% 
  dplyr::glimpse()
```

```
Rows: 2,550
Columns: 7
$ Year           <dbl> 2014, 2014, 2014, 2014, 2014, 2014, 2014, 2014, 2014...
$ Family         <chr> "Acanthuridae", "Ariidae", "Caesionidae", "Carangida...
$ Eng_name       <chr> "Naso hexacanthus", "Netuma thalassina", "Caesio xan...
$ `Swahil-_name` <chr> "Puju", "Hongwe", "Mbono", "Karambizi", "Kambale", "...
$ Districts      <chr> "Muheza", "Muheza", "Muheza", "Muheza", "Muheza", "M...
$ `Catch(t)`     <chr> "7.1", "50.7", "2", "136.2", "1.1", "16.1", "1.3", "...
$ Zone           <chr> "OTHER", "OTHER", "OTHER", "OTHER", "OTHER", "OTHER"...
```

## Missing values

Next, let’s check our data frame to determine if there are any missing values by subsetting observations (rows) in our dataframe that have missing values using the `filter` function and the logical operator for negation, `!` . We found that there are seven observations without catch data---with missing values.


```
# A tibble: 9 x 7
   Year Family       Eng_name      `Swahil-_name`     Districts `Catch(t)` Zone 
  <dbl> <chr>        <chr>         <chr>              <chr>     <chr>      <chr>
1  2017 Scombridae ~ Thunnus obes~ Jodari macho maku~ Muheza    <NA>       OTHER
2  2017 Scombridae ~ Thunnus obes~ Jodari macho maku~ Pangani   <NA>       OTHER
3  2017 Scombridae ~ Thunnus obes~ Jodari macho maku~ Tanga     <NA>       OTHER
4  2017 Scombridae ~ Thunnus obes~ Jodari macho maku~ Mkinga    <NA>       OTHER
5    NA <NA>         <NA>          <NA>               <NA>      <NA>       <NA> 
6    NA <NA>         <NA>          <NA>               <NA>      <NA>       <NA> 
7    NA <NA>         <NA>          <NA>               <NA>      <NA>       <NA> 
8    NA <NA>         <NA>          <NA>               <NA>      <NA>       <NA> 
9    NA <NA>         <NA>          <NA>               <NA>      <NA>       <NA> 
```
We can drop this observations from our dataset using the `drop_na` function from **dplyr** package. This function remove any rows with missing values from our dataset:


```r
fisheries.data %>%
  drop_na()
```

Checking the data structure again, we can see that the 3 rows containing NA values have been removed from our dataframe. You may not always wish to remove NA values from a dataset, if you still want to keep other values in that observation. Even if you want to keep observations with NA values in the dataset, it is still good to identify NAs and know where they occur to ensure they don’t create problems during analyses.

## Typos

We can check for typos by using the unique function, which will tell us all of the unique values found within a particular column. As an example, let’s look at the Gear variable.


```
 [1] "Bagamoyo"     "Ilala"        "Kibiti"       "Kigamboni"    "Kilwa"       
 [6] "Kindondoni"   "Kinondoni"    "Lindi"        "Lindi Rural"  "Lindi urban" 
[11] "Lindi Urban"  "Mafia"        "Mkinga"       "Mkuranga"     "Mtwara"      
[16] "Mtwara Rural" "Mtwara Urban" "Muheza"       "Pangani"      "Rufiji"      
[21] "Tanga"        "Temeke"       NA            
```

The District variable has 23 unique values, however, we notice that know there should only be 6 gears present in the dataset. We can see that `Kinondoni` appears twice because typo. `Kindondoni` and   `Kinondoni`. We fixed this by replacing the `Kindondoni` with `Kinondoni`.



## Errors
We also checked for any catch data values errors  values may be caused from typos during data entry. To look at the range and distribution of a numeric variable, the summary function can be used.


```
-------------------------------- Variable: catch -------------------------------

                             Univariate Analysis                               

 N                          2550.00      Variance                 213912.10 
 Missing                       9.00      Std Deviation               462.51 
 Mean                        741.56      Range                      1547.00 
 Median                      763.00      Interquartile Range         806.00 
 Mode                         31.00      Uncorrected SS       1940671613.00 
 Trimmed Mean                739.63      Corrected SS          543336727.24 
 Skewness                     -0.01      Coeff Variation              62.37 
 Kurtosis                     -1.23      Std Error Mean                9.18 

                                   Quantiles                                    

                Quantile                                  Value                  

               Max                                       1548.00                 
               99%                                       1530.60                 
               95%                                       1459.00                 
               90%                                       1370.00                 
               Q3                                        1144.00                 
               Median                                    763.00                  
               Q1                                        338.00                  
               10%                                        78.00                  
               5%                                         31.00                  
               1%                                         7.00                   
               Min                                        1.00                   

                                 Extreme Values                                 

                   Low                                    High                   

  Obs                             Value       Obs                             Value 
 1993                               1        1232                             1548  
 1994                               1         73                              1547  
 2027                               1        1528                             1546  
 2059                               1        1551                             1546  
 2060                               1        1947                             1546  
```

Looks like we have a max landing value that is order of magnitude higher than the mean and median values. Visualizing numeric data is another great way to screen continuous data and identify data outlines that may be caused from errors in the dataset. We can clearly see  from figure \@ref(fig:fig71) that the dataset has an outlier (upper left corner of the plot). We are not sure how this error occurred, but we know that this is not correct. In fact, we need to check to find out whether this are actual landing or typo error occured during data entry.

![(\#fig:fig71)Fisheries data with a) outlier and b) transformed that remove outliers](07-appendix_files/figure-latex/fig71-1.pdf) 



\begin{landscape}\begingroup\fontsize{10}{12}\selectfont

\begin{longtable}[t]{r>{\raggedright\arraybackslash}p{1.2in}>{\raggedright\arraybackslash}p{1.2in}>{\raggedright\arraybackslash}p{1.2in}>{\raggedright\arraybackslash}p{1.2in}>{\raggedright\arraybackslash}p{1.2in}>{\raggedright\arraybackslash}p{1.2in}}
\caption{(\#tab:tab0)Sampled 100 observation of the fisheries catch in RUMAKI and Non--RUMAKI Areas between 2014 and 2018}\\
\toprule
Year & Zone & Districts & Family & English Name & Swahili Name & Catch\\
\midrule
\endfirsthead
\caption[]{(\#tab:tab0)Sampled 100 observation of the fisheries catch in RUMAKI and Non--RUMAKI Areas between 2014 and 2018 \textit{(continued)}}\\
\toprule
Year & Zone & Districts & Family & English Name & Swahili Name & Catch\\
\midrule
\endhead
\
\endfoot
\bottomrule
\endlastfoot
 &  & Ilala & Scombridae(N) & Scomberomorus plurilineatus & Nguru- kanadi & 201.1\\

 &  & Ilala & Serranidae & Cephalopholis argus & Chewa & 490.7\\

 &  & Mkinga & Haemulidae & Pomadasys multimaculatum & Karamamba & 13.2\\

 &  & Mkuranga & Loliginidae & Uroteuthis duvaucelii & Ngisi & 0.2\\

 &  & Pangani & Clariidae & clarias macrocephalus & Kambale & 5.3\\

 &  & Tanga & Clariidae & clarias macrocephalus & Kambale & 1.1\\

 &  & Tanga & Penaeidae & Penaeus Monodon & Kamba mti & 0.4\\

 &  & Temeke & Siganidae & Siganus Canaliculatus & Tasi & 107\\

 & \multirow{-9}{1.2in}{\raggedright\arraybackslash OTHER} & Temeke & Istiophoridae & Istiompax indica & Samsuri & 172.7\\

 &  & Kilwa & Nemipteridae & Nemipterus japonicus & Koana & 0.3\\

 &  & Kilwa & Gerridae & Gerres oblongus & Chaa & 0.2\\

 &  & Kilwa & Dasyatidae/ Rays & Pastinachus sephen & Taa usinga & 125.1\\

 &  & Mtwara & Serranidae & Cephalopholis argus & Chewa & 61\\

 &  & Mtwara & Haemulidae & Pomadasys multimaculatum & Karamamba & 0.1\\

\multirow{-15}{*}{\raggedleft\arraybackslash 2014} & \multirow{-6}{1.2in}{\raggedright\arraybackslash RUMAKI} & Mtwara & Dasyatidae/ Rays & Pastinachus sephen & Taa usinga & 270\\
\cmidrule{1-7}
 &  & Bagamoyo & Octopodidae & Octopus cyanea & Pweza & 39.4\\

 &  & Bagamoyo & Sphyraenidae & Sphyraena obtusata & Msusa, Mzia & 82.4\\

 &  & Ilala & Sphyraenidae & Sphyraena obtusata & Msusa, Mzia & 312.4\\

 &  & Ilala & Serranidae & Cephalopholis argus & Chewa & 581.3\\

 &  & Lindi & Lethrinidae & Lethrinus harak & Changu doa & 39.5\\

 &  & Mkinga & Chirocentridae & Chirocentrus nudus & Mkonge & 33.6\\

 &  & Pangani & Lethrinidae & Lethrinus harak & Changu doa & 330.6\\

 &  & Tanga & Clupeidae & Sardinella neglecta & Dagaa papa & 1344.7\\

 &  & Tanga & Clariidae & clarias macrocephalus & Kambale & 3.6\\

 &  & Temeke & Siganidae & Siganus Canaliculatus & Tasi & 155.4\\

 & \multirow{-11}{1.2in}{\raggedright\arraybackslash OTHER} & Temeke & Palinuridae & Panulirus ornatus & Kamba koche & 1\\

 &  & Kilwa & Ariidae & Netuma thalassina & Hongwe & 3.2\\

 &  & Mafia & Haemulidae & Pomadasys multimaculatum & Karamamba & 3.5\\

 &  & Mafia & Scombridae(N) & Scomberomorus plurilineatus & Nguru- kanadi & 121\\

 &  & Mafia & Octopodidae & Octopus cyanea & Pweza & 114.5\\

 &  & Mtwara & Penaeidae & Penaeus Monodon & Kamba mti & 2.1\\

 &  & Rufiji & Loliginidae & Uroteuthis duvaucelii & Ngisi & 2.6\\

\multirow{-18}{*}{\raggedleft\arraybackslash 2015} & \multirow{-7}{1.2in}{\raggedright\arraybackslash RUMAKI} & Rufiji & Labridae & Halichoeres nigrescens & pono & 4.3\\
\cmidrule{1-7}
 &  & Bagamoyo & Clariidae & clarias macrocephalus & Kambale & 1.84\\

 &  & Bagamoyo & Lethrinidae & Lethrinus harak & Changu doa & 9.64\\

 &  & Ilala & Scombridae (v) & Rastrelliger kanagurta & Vibua & 688.04\\

 &  & Ilala & Octopodidae & Octopus cyanea & Pweza & 301.14\\

 &  & Ilala & Acanthuridae & Naso hexacanthus & Puju & 8.84\\

 &  & Lindi & Palinuridae & Panulirus ornatus & Kamba koche & 0.78\\

 &  & Lindi & Scombridae(N) & Scomberomorus plurilineatus & Nguru- kanadi & 16.34\\

 &  & Lindi & Mullidae & Upeneus tragula & Mkundaji & 5.22\\

 &  & Lindi & Chirocentridae & Chirocentrus nudus & Mkonge & 25.18\\

 &  & Lindi & Rachycentridae & Rachycentron canadum & Songoro & 89.18\\

 &  & Lindi Rural & Palinuridae & Panulirus ornatus & Kamba koche & 1.17\\

 &  & Mkinga & Acanthuridae & Naso hexacanthus & Puju & 7.94\\

 &  & Mkinga & Istiophoridae & Istiompax indica & Samsuri & 26.84\\

 &  & Mkuranga & Labridae & Halichoeres nigrescens & pono & 1.04\\

 &  & Mkuranga & Lethrinidae & Lethrinus harak & Changu doa & 103.84\\

 &  & Muheza & Nemipteridae & Nemipterus japonicus & Koana & 82.64\\

 &  & Muheza & Ariidae & Netuma thalassina & Hongwe & 47.84\\

 &  & Pangani & Carangidae & carenx tille & Karambizi & 91.54\\

 & \multirow{-19}{1.2in}{\raggedright\arraybackslash OTHER} & Temeke & Dasyatidae/ Rays & Pastinachus sephen & Taa usinga & 12.56\\

 &  & Kilwa & Loliginidae & Uroteuthis duvaucelii & Ngisi & 0.94\\

 &  & Mafia & Scombridae(N) & Scomberomorus plurilineatus & Nguru- kanadi & 120.44\\

 &  & Mafia & Chirocentridae & Chirocentrus nudus & Mkonge & 65.64\\

 &  & Mafia & Hemiramphidae & Hemiramphus far & Chuchunge & 62.64\\

 &  & Mtwara & Penaeidae & Penaeus Monodon & Kamba mti & 0.37\\

\multirow{-25}{*}{\raggedleft\arraybackslash 2016} & \multirow{-6}{1.2in}{\raggedright\arraybackslash RUMAKI} & Mtwara & Scombridae(J) / & Gymnosarda unicolor & Jodari & 17.41\\
\cmidrule{1-7}
 &  & Ilala & Serranidae & Cephalopholis argus & Chewa & 4.39\\

 &  & Ilala & Chanidae & Chanidae & Mwatiko & 221.49\\

 &  & Kinondoni & Scombridae(N) & Scomberomorus plurilineatus & Nguru- kanadi & 219.09\\

 &  & Lindi urban & Chirocentridae & Chirocentrus nudus & Mkonge & 27.52\\

 &  & Lindi urban & Haemulidae & Pomadasys multimaculatum & Karamamba & 89.92\\

 &  & Lindi urban & Rachycentridae & Rachycentron canadum & Songoro & 4.2\\

 &  & Mkinga & Scombridae(N) & Scomberomorus plurilineatus & Nguru- kanadi & 106.5\\

 &  & Mkinga & Acanthuridae & Naso hexacanthus & Puju & 8.5\\

 &  & Mkinga & Carangidae & carenx tille & Karambizi & 132.2\\

 &  & Mkinga & Carcharhinidae /Sharks & Carcharhinus falciformis & Papa & 117.6\\

 &  & Mkuranga & Carcharhinidae /Sharks & Carcharhinus falciformis & Papa & 274.49\\

 &  & Muheza & Mullidae & Upeneus tragula & Mkundaji & 5\\

 &  & Pangani & Scombridae(J) & Gymnosarda unicolor & Jodari  meno & 3.1\\

 &  & Pangani & Nemipteridae & Nemipterus japonicus & Koana & 53.5\\

 &  & Pangani & Carangidae & carenx tille & Karambizi & 92.1\\

 &  & Pangani & Scombridae (v) & Rastrelliger kanagurta & Vibua & 3.1\\

 &  & Pangani & Mullidae & Upeneus tragula & Mkundaji & 2\\

 &  & Pangani & Acanthuridae & Naso hexacanthus & Puju & 5.2\\

 &  & Tanga & Scombridae (v) & Rastrelliger kanagurta & Vibua & 569.4\\

 &  & Temeke & Palinuridae & Panulirus ornatus & Kamba koche & 581.09\\

 &  & Temeke & Labridae & Halichoeres nigrescens & pono & 26.99\\

 & \multirow{-22}{1.2in}{\raggedright\arraybackslash OTHER} & Temeke & Scombridae (J) & Thunnus obesus & Jodari macho makubwa & 203.39\\

 &  & Kibiti & Mullidae & Upeneus tragula & Mkundaji & 4.29\\

 &  & Kilwa & Nemipteridae & Nemipterus japonicus & Koana & 4.89\\

 &  & Mtwara Rural & Clariidae & clarias macrocephalus & Kambale & 3.87\\

\multirow{-26}{*}{\raggedleft\arraybackslash 2017} & \multirow{-4}{1.2in}{\raggedright\arraybackslash RUMAKI} & Mtwara Urban & Chanidae & Chanidae & Mwatiko & 2.52\\
\cmidrule{1-7}
 &  & Kinondoni & Octopodidae & Octopus cyanea & Pweza & 476.61\\

 &  & Kinondoni & Istiophoridae & Istiompax indica & Samsuri & 12.11\\

 &  & Lindi Rural & Lethrinidae & Lethrinus harak & Changu doa & 4.11\\

 &  & Lindi Urban & Chirocentridae & Chirocentrus nudus & Mkonge & 25.85\\

 &  & Lindi Urban & Clariidae & clarias macrocephalus & Kambale & 5.29\\

 &  & Muheza & Siganidae & Siganus Canaliculatus & Tasi & 77.51\\

 &  & Pangani & Labridae & Halichoeres nigrescens & pono & 4.21\\

 & \multirow{-8}{1.2in}{\raggedright\arraybackslash OTHER} & Temeke & Clupeidae & Sardinella neglecta & Dagaa papa & 10.41\\

 &  & Kigamboni & Chirocentridae & Chirocentrus nudus & Mkonge & 0.67\\

 &  & Kigamboni & Loliginidae & Uroteuthis duvaucelii & Ngisi & 132.31\\

 &  & Kilwa & Caesionidae & Caesio xanthonota & Mbono & 55.21\\

 &  & Mafia & Acanthuridae & Naso hexacanthus & Puju & 0.67\\

 &  & Mafia & Octopodidae & Panulirus ornatus & Kamba koche & 114.61\\

 &  & Mtwara Urban & Scombridae & Euthynus affinis & Sehewa & 46.21\\

 &  & Mtwara Urban & Dasyatidae/ Rays & Pastinachus sephen & Taa usinga & 231.71\\

\multirow{-16}{*}{\raggedleft\arraybackslash 2018} & \multirow{-8}{1.2in}{\raggedright\arraybackslash RUMAKI} & Mtwara Urban & Caesionidae & Caesio xanthonota & Mbono & 2.02\\*
\end{longtable}
\endgroup{}
\end{landscape}


We can look at the raw data just by typing landings_data.

## Annual landings

The first analysis we performed while preparing this document is calculating annual landings in the fishery and compute the summary by Year, Zone and Priority groups. To calculate annual landings, take your landings_data data frame, add a column for `catch` of individual fish in kilograms by using the `mutate` function, group the data by year by using the `group_by` function, and then `summarize` the data for each year by summing the total weight of all fish caught in each year, zone and priority group using the `summarize` and sum functions. All these function are imported from **dplyr** package [@R-dplyr], which is part of the **tidyverse** ecosystem [@R-tidyverse]. 


```r
load("data/fish_catch_statistics.RData")

catch.stats = catch.clean.group.outlierfree %>% 
  group_by(Year,Zone, group) %>% 
  summarise(n = n(),
            catch.mean = mean(catch, na.rm = TRUE),
            catch.median = median(catch, na.rm = TRUE),
            catch.sd = sd(catch, na.rm = TRUE),
            catch.se = catch.sd/sqrt(n),
            catch.rate = catch.median/n,
            catch.rate.se = catch.se/n) %>%
  ungroup() 
```
Note the use of `na.rm = TRUE` in the code above. This is an important argument of many R functions `(median() in this case)` and it tells R what to do with NA values in your data. Here, we are telling R to first remove NA values before calculating the mean, median, and standard error. By default, many functions will return `NA` if any value is `NA`, which is often not desirable.The total landings for the five major groups as shown in appendix \@ref(tab:tab07).



\begin{landscape}\begingroup\fontsize{10}{12}\selectfont

\begin{longtable}[t]{r>{\raggedright\arraybackslash}p{.75in}>{\raggedright\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}}
\caption{(\#tab:tab07)Total Annual fish catch and catch rate for priority species in RUMAKI and Non--RUMAKI Areas}\\
\toprule
\multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{4}{c}{Total Fish Catch (Tones)} & \multicolumn{2}{c}{Catch Rate} \\
\cmidrule(l{3pt}r{3pt}){4-7} \cmidrule(l{3pt}r{3pt}){8-9}
Year & Zone & Group & Mean & Median & SD & SE & Median & SE\\
\midrule
\endfirsthead
\caption[]{(\#tab:tab07)Total Annual fish catch and catch rate for priority species in RUMAKI and Non--RUMAKI Areas \textit{(continued)}}\\
\toprule
\multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{4}{c}{Total Fish Catch (Tones)} & \multicolumn{2}{c}{Catch Rate} \\
\cmidrule(l{3pt}r{3pt}){4-7} \cmidrule(l{3pt}r{3pt}){8-9}
Year & Zone & Group & Mean & Median & SD & SE & Median & SE\\
\midrule
\endhead
\
\endfoot
\bottomrule
\endlastfoot
 &  & Octopus & 127.34 & 72.75 & 117.92 & 37.29 & 7.28 & 3.73\\

 &  & Reef fish & 174.71 & 80.40 & 145.84 & 55.12 & 11.49 & 7.87\\

 &  & Small pelagic & 121.21 & 137.30 & 92.27 & 34.87 & 19.61 & 4.98\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash OTHER} & Tuna fish & 106.09 & 103.95 & 88.18 & 27.88 & 10.39 & 2.79\\

 &  & Octopus & 59.28 & 58.85 & 62.77 & 31.39 & 14.71 & 7.85\\

 &  & Reef fish & 211.17 & 229.15 & 130.67 & 65.33 & 57.29 & 16.33\\

 &  & Small pelagic & 138.75 & 41.30 & 223.12 & 111.56 & 10.32 & 27.89\\

\multirow{-8}{*}{\raggedleft\arraybackslash 2014} & \multirow{-4}{.75in}{\raggedright\arraybackslash RUMAKI} & Tuna fish & 68.70 & 49.45 & 85.03 & 42.51 & 12.36 & 10.63\\
\cmidrule{1-9}
 &  & Octopus & 105.87 & 75.10 & 92.77 & 29.34 & 7.51 & 2.93\\

 &  & Reef fish & 160.12 & 104.40 & 135.44 & 45.15 & 11.60 & 5.02\\

 &  & Small pelagic & 164.78 & 172.20 & 113.22 & 46.22 & 28.70 & 7.70\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash OTHER} & Tuna fish & 100.53 & 102.95 & 74.25 & 21.43 & 8.58 & 1.79\\

 &  & Octopus & 120.10 & 62.40 & 164.30 & 82.15 & 15.60 & 20.54\\

 &  & Reef fish & 158.25 & 123.00 & 152.55 & 76.27 & 30.75 & 19.07\\

 &  & Small pelagic & 149.90 & 126.35 & 131.50 & 65.75 & 31.59 & 16.44\\

\multirow{-8}{*}{\raggedleft\arraybackslash 2015} & \multirow{-4}{.75in}{\raggedright\arraybackslash RUMAKI} & Tuna fish & 69.02 & 107.20 & 60.97 & 27.27 & 21.44 & 5.45\\
\cmidrule{1-9}
 &  & Octopus & 87.40 & 49.44 & 91.52 & 27.60 & 4.49 & 2.51\\

 &  & Reef fish & 113.46 & 60.34 & 124.62 & 39.41 & 6.03 & 3.94\\

 &  & Small pelagic & 158.92 & 190.38 & 114.35 & 40.43 & 23.80 & 5.05\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash OTHER} & Tuna fish & 83.03 & 66.94 & 78.08 & 21.66 & 5.15 & 1.67\\

 &  & Octopus & 94.71 & 48.76 & 136.13 & 55.58 & 8.13 & 9.26\\

 &  & Reef fish & 114.80 & 85.19 & 86.63 & 38.74 & 17.04 & 7.75\\

 &  & Small pelagic & 109.93 & 93.37 & 70.26 & 28.68 & 15.56 & 4.78\\

\multirow{-8}{*}{\raggedleft\arraybackslash 2016} & \multirow{-4}{.75in}{\raggedright\arraybackslash RUMAKI} & Tuna fish & 63.65 & 63.99 & 54.46 & 20.58 & 9.14 & 2.94\\
\cmidrule{1-9}
 &  & Octopus & 145.62 & 97.20 & 146.01 & 44.02 & 8.84 & 4.00\\

 &  & Reef fish & 76.30 & 52.20 & 103.81 & 34.60 & 5.80 & 3.84\\

 &  & Small pelagic & 160.60 & 191.83 & 114.73 & 40.56 & 23.98 & 5.07\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash OTHER} & Tuna fish & 56.35 & 18.68 & 73.11 & 14.62 & 0.75 & 0.58\\

 &  & Octopus & 65.17 & 68.09 & 45.97 & 22.98 & 17.02 & 5.75\\

 &  & Reef fish & 60.56 & 38.69 & 73.06 & 29.83 & 6.45 & 4.97\\

 &  & Small pelagic & 112.27 & 95.71 & 70.26 & 28.68 & 15.95 & 4.78\\

\multirow{-8}{*}{\raggedleft\arraybackslash 2017} & \multirow{-4}{.75in}{\raggedright\arraybackslash RUMAKI} & Tuna fish & 47.88 & 33.29 & 47.13 & 12.17 & 2.22 & 0.81\\
\cmidrule{1-9}
 &  & Octopus & 156.43 & 95.53 & 141.74 & 42.74 & 8.68 & 3.89\\

 &  & Reef fish & 63.69 & 21.26 & 77.01 & 24.35 & 2.13 & 2.44\\

 &  & Small pelagic & 159.59 & 191.05 & 114.35 & 40.43 & 23.88 & 5.05\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash OTHER} & Tuna fish & 66.03 & 25.16 & 80.48 & 14.69 & 0.84 & 0.49\\

 &  & Octopus & 30.65 & 12.61 & 34.60 & 15.47 & 2.52 & 3.09\\

 &  & Reef fish & 26.11 & 20.71 & 23.56 & 9.62 & 3.45 & 1.60\\

 &  & Small pelagic & 110.59 & 94.03 & 70.26 & 28.68 & 15.67 & 4.78\\

\multirow{-8}{*}{\raggedleft\arraybackslash 2018} & \multirow{-4}{.75in}{\raggedright\arraybackslash RUMAKI} & Tuna fish & 81.09 & 49.01 & 98.17 & 24.54 & 3.06 & 1.53\\*
\end{longtable}
\endgroup{}
\end{landscape}

## Mean annual catch 
The other paramater we computed is the grand mean of catch for RUMAKI and Non-RUMAKI areas. The aim is to understand the performance of fishery for fishing activities within and outside the RUMAKI area and assess how they vary for the priority fish groups. In this we group the data frame by both the Zone and group in order to summarize the total landings by fishing zone and by the priority fish group. 


```r
catch.stats.overall = catch.clean.group.outlierfree %>% 
  group_by(Zone, group) %>% 
  summarise(n = n(),
            catch.mean = mean(catch, na.rm = TRUE),
            catch.median = median(catch, na.rm = TRUE),
            catch.sd = sd(catch, na.rm = TRUE),
            catch.se = catch.sd/sqrt(n),
            catch.rate = catch.median/n,
            catch.rate.se = catch.se/n) %>%
  ungroup()
```



\begin{landscape}\begingroup\fontsize{10}{12}\selectfont

\begin{longtable}[t]{l>{\raggedright\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}}
\caption{(\#tab:tab071)Average Total fish catch and catch rate for priority species in RUMAKI and Non--RUMAKI Areas}\\
\toprule
\multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{4}{c}{Total Fish Catch (Tones)} & \multicolumn{2}{c}{Catch Rate} \\
\cmidrule(l{3pt}r{3pt}){3-6} \cmidrule(l{3pt}r{3pt}){7-8}
Zone & Group & Mean & Median & SD & SE & Median & SE\\
\midrule
\endfirsthead
\caption[]{(\#tab:tab071)Average Total fish catch and catch rate for priority species in RUMAKI and Non--RUMAKI Areas \textit{(continued)}}\\
\toprule
\multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{4}{c}{Total Fish Catch (Tones)} & \multicolumn{2}{c}{Catch Rate} \\
\cmidrule(l{3pt}r{3pt}){3-6} \cmidrule(l{3pt}r{3pt}){7-8}
Zone & Group & Mean & Median & SD & SE & Median & SE\\
\midrule
\endhead
\
\endfoot
\bottomrule
\endlastfoot
 & Octopus & 124.83 & 83.31 & 118.91 & 16.33 & 1.57 & 0.31\\

 & Reef fish & 113.83 & 69.60 & 120.30 & 17.93 & 1.55 & 0.40\\

 & Small pelagic & 153.25 & 161.56 & 105.32 & 17.32 & 4.37 & 0.47\\

\multirow{-4}{*}{\raggedright\arraybackslash OTHER} & Tuna fish & 74.85 & 54.91 & 78.56 & 8.28 & 0.61 & 0.09\\
\cmidrule{1-8}
 & Octopus & 73.90 & 51.21 & 99.60 & 20.77 & 2.23 & 0.90\\

 & Reef fish & 102.87 & 44.09 & 109.26 & 21.85 & 1.76 & 0.87\\

 & Small pelagic & 121.21 & 76.23 & 106.15 & 20.82 & 2.93 & 0.80\\

\multirow{-4}{*}{\raggedright\arraybackslash RUMAKI} & Tuna fish & 65.55 & 47.89 & 72.03 & 10.51 & 1.02 & 0.22\\*
\end{longtable}
\endgroup{}
\end{landscape}




\begin{landscape}\begingroup\fontsize{10}{12}\selectfont

\begin{longtable}[t]{l>{\raggedright\arraybackslash}p{.75in}>{\raggedright\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}>{\raggedleft\arraybackslash}p{.75in}}
\caption{(\#tab:tab072)Average Total fish catch and catch rate for priority species for districts within and outside in RUMAKI seascape}\\
\toprule
\multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{4}{c}{Total Fish Catch (Tones)} & \multicolumn{2}{c}{Catch Rate} \\
\cmidrule(l{3pt}r{3pt}){4-7} \cmidrule(l{3pt}r{3pt}){8-9}
Zone & District & Group & Mean & Median & SD & SE & Median & SE\\
\midrule
\endfirsthead
\caption[]{(\#tab:tab072)Average Total fish catch and catch rate for priority species for districts within and outside in RUMAKI seascape \textit{(continued)}}\\
\toprule
\multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{1}{c}{} & \multicolumn{4}{c}{Total Fish Catch (Tones)} & \multicolumn{2}{c}{Catch Rate} \\
\cmidrule(l{3pt}r{3pt}){4-7} \cmidrule(l{3pt}r{3pt}){8-9}
Zone & District & Group & Mean & Median & SD & SE & Median & SE\\
\midrule
\endhead
\
\endfoot
\bottomrule
\endlastfoot
 &  & Octopus & 75.17 & 39.40 & 94.04 & 42.06 & 7.88 & 8.41\\

 &  & Reef fish & 64.77 & 11.99 & 89.72 & 40.12 & 2.40 & 8.02\\

 & \multirow{-3}{.75in}{\raggedright\arraybackslash Bagamoyo} & Tuna fish & 99.19 & 111.72 & 32.49 & 9.38 & 9.31 & 0.78\\

 &  & Octopus & 302.31 & 301.70 & 1.94 & 0.87 & 60.34 & 0.17\\

 &  & Reef fish & 218.23 & 330.54 & 199.93 & 89.41 & 66.11 & 17.88\\

 & \multirow{-3}{.75in}{\raggedright\arraybackslash Ilala} & Tuna fish & 150.60 & 201.10 & 99.21 & 33.07 & 22.34 & 3.67\\

 &  & Octopus & 222.03 & 55.80 & 233.18 & 104.28 & 11.16 & 20.86\\

 &  & Reef fish & 137.69 & 137.97 & 43.12 & 21.56 & 34.49 & 5.39\\

 &  & Small pelagic & 250.17 & 253.10 & 7.36 & 3.29 & 50.62 & 0.66\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Kinondoni} & Tuna fish & 131.64 & 217.41 & 119.57 & 39.86 & 24.16 & 4.43\\

 &  & Octopus & 178.55 & 222.21 & 77.08 & 44.50 & 74.07 & 14.83\\

 &  & Reef fish & 11.09 & 5.79 & 10.67 & 6.16 & 1.93 & 2.05\\

 &  & Small pelagic & 239.83 & 239.50 & 1.21 & 0.70 & 79.83 & 0.23\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Lindi Rural} & Tuna fish & 14.58 & 16.31 & 7.55 & 3.38 & 3.26 & 0.68\\

 &  & Octopus & 91.49 & 95.53 & 38.00 & 16.99 & 19.11 & 3.40\\

 &  & Reef fish & 23.87 & 15.58 & 22.83 & 10.21 & 3.12 & 2.04\\

 &  & Small pelagic & 170.24 & 160.72 & 20.06 & 10.03 & 40.18 & 2.51\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Lindi Urban} & Tuna fish & 18.70 & 18.01 & 13.03 & 4.34 & 2.00 & 0.48\\

 &  & Octopus & 123.79 & 116.20 & 11.45 & 5.12 & 23.24 & 1.02\\

 &  & Reef fish & 87.29 & 69.60 & 34.17 & 15.28 & 13.92 & 3.06\\

 &  & Small pelagic & 85.01 & 81.50 & 8.10 & 3.62 & 16.30 & 0.72\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Mkinga} & Tuna fish & 113.81 & 106.50 & 18.82 & 7.68 & 17.75 & 1.28\\

 &  & Octopus & 2.71 & 2.71 & 1.64 & 0.73 & 0.54 & 0.15\\

 &  & Reef fish & 103.07 & 104.40 & 69.87 & 31.25 & 20.88 & 6.25\\

 &  & Small pelagic & 276.87 & 303.30 & 59.86 & 26.77 & 60.66 & 5.35\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Mkuranga} & Tuna fish & 30.43 & 3.02 & 78.83 & 27.87 & 0.38 & 3.48\\

 &  & Octopus & 97.93 & 44.00 & 100.40 & 44.90 & 8.80 & 8.98\\

 &  & Reef fish & 16.31 & 16.31 & NA & NA & 16.31 & NA\\

 &  & Small pelagic & 148.74 & 221.82 & 113.47 & 46.33 & 36.97 & 7.72\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Muheza} & Tuna fish & 65.05 & 59.92 & 74.97 & 26.51 & 7.49 & 3.31\\

 &  & Octopus & 195.63 & 211.40 & 86.57 & 38.71 & 42.28 & 7.74\\

 &  & Reef fish & 285.89 & 330.04 & 93.95 & 42.02 & 66.01 & 8.40\\

 &  & Small pelagic & 6.29 & 6.40 & 0.30 & 0.15 & 1.60 & 0.04\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Pangani} & Tuna fish & 11.36 & 5.82 & 19.81 & 7.01 & 0.73 & 0.88\\

 &  & Octopus & 51.09 & 42.90 & 14.64 & 6.55 & 8.58 & 1.31\\

 &  & Reef fish & 50.41 & 52.20 & 15.57 & 6.96 & 10.44 & 1.39\\

 & \multirow{-3}{.75in}{\raggedright\arraybackslash Tanga} & Tuna fish & 50.73 & 67.50 & 35.02 & 13.24 & 9.64 & 1.89\\

 &  & Octopus & 53.95 & 36.79 & 39.31 & 17.58 & 7.36 & 3.52\\

 &  & Reef fish & 177.20 & 177.20 & 213.54 & 150.99 & 88.60 & 75.50\\

 &  & Small pelagic & 58.37 & 12.08 & 65.43 & 29.26 & 2.42 & 5.85\\

\multirow{-41}{*}{\raggedright\arraybackslash OTHER} & \multirow{-4}{.75in}{\raggedright\arraybackslash Temeke} & Tuna fish & 96.87 & 50.19 & 77.15 & 25.72 & 5.58 & 2.86\\
\cmidrule{1-9}
 &  & Octopus & 1.07 & 1.07 & 0.33 & 0.24 & 0.54 & 0.12\\

 &  & Reef fish & 22.88 & 30.94 & 16.04 & 9.26 & 10.31 & 3.09\\

 &  & Small pelagic & 51.55 & 51.21 & 1.21 & 0.70 & 17.07 & 0.23\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Kibiti} & Tuna fish & 1.94 & 2.35 & 0.76 & 0.34 & 0.47 & 0.07\\

 &  & Octopus & 85.52 & 83.29 & 5.39 & 3.11 & 27.76 & 1.04\\

 &  & Reef fish & 63.15 & 63.15 & 1.19 & 0.84 & 31.58 & 0.42\\

 &  & Small pelagic & 113.01 & 112.67 & 1.21 & 0.70 & 37.56 & 0.23\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Kigamboni} & Tuna fish & 94.86 & 71.39 & 43.17 & 16.32 & 10.20 & 2.33\\

 &  & Octopus & 186.17 & 118.90 & 155.55 & 69.56 & 23.78 & 13.91\\

 &  & Reef fish & 67.67 & 43.84 & 92.45 & 41.35 & 8.77 & 8.27\\

 &  & Small pelagic & 161.65 & 201.60 & 90.09 & 40.29 & 40.32 & 8.06\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Kilwa} & Tuna fish & 10.50 & 6.09 & 12.66 & 4.22 & 0.68 & 0.47\\

 &  & Octopus & 93.05 & 113.94 & 45.08 & 20.16 & 22.79 & 4.03\\

 &  & Reef fish & 173.29 & 201.60 & 79.62 & 35.61 & 40.32 & 7.12\\

 &  & Small pelagic & 34.31 & 35.00 & 2.46 & 1.10 & 7.00 & 0.22\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Mafia} & Tuna fish & 147.24 & 120.72 & 94.54 & 29.90 & 12.07 & 2.99\\

 &  & Octopus & 7.48 & 9.80 & 4.46 & 2.57 & 3.27 & 0.86\\

 &  & Reef fish & 263.96 & 351.20 & 154.84 & 89.40 & 117.07 & 29.80\\

 &  & Small pelagic & 286.17 & 311.90 & 199.84 & 115.38 & 103.97 & 38.46\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Mtwara} & Tuna fish & 77.03 & 98.30 & 44.77 & 25.85 & 32.77 & 8.62\\

 &  & Octopus & 5.85 & 5.85 & NA & NA & 5.85 & NA\\

 &  & Reef fish & 99.82 & 44.09 & 97.99 & 56.58 & 14.70 & 18.86\\

 &  & Small pelagic & 187.81 & 187.48 & 1.21 & 0.70 & 62.49 & 0.23\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Mtwara Rural} & Tuna fish & 39.30 & 27.89 & 16.77 & 6.34 & 3.98 & 0.91\\

 &  & Octopus & 7.35 & 7.35 & 1.19 & 0.84 & 3.67 & 0.42\\

 &  & Reef fish & 6.95 & 6.95 & 1.19 & 0.84 & 3.48 & 0.42\\

 &  & Small pelagic & 76.23 & 76.23 & 1.19 & 0.84 & 38.11 & 0.42\\

 & \multirow{-4}{.75in}{\raggedright\arraybackslash Mtwara Urban} & Tuna fish & 82.95 & 82.95 & 41.47 & 20.73 & 20.74 & 5.18\\

 &  & Octopus & 0.95 & 0.95 & 0.64 & 0.45 & 0.48 & 0.22\\

 &  & Reef fish & 33.35 & 33.35 & 2.62 & 1.85 & 16.68 & 0.93\\

 &  & Small pelagic & 51.75 & 51.75 & 0.92 & 0.65 & 25.88 & 0.32\\

\multirow{-32}{*}{\raggedright\arraybackslash RUMAKI} & \multirow{-4}{.75in}{\raggedright\arraybackslash Rufiji} & Tuna fish & 1.25 & 1.25 & 1.63 & 1.15 & 0.62 & 0.57\\*
\end{longtable}
\endgroup{}
\end{landscape}




## Calculating Catch-per-Unit-Effort (CPUE)

To compare catch between RUMAKI and Non--RUMAKI area we first standardized them by calculate the catch per unit effort. We also computed catch-per-unit-effort (CPUE). CPUE is calculated by dividing the catch of each fishing trip by the number of hours fished during that trip. This gives CPUE in units of kilograms per hour. The median for every year is then calculated in order to remove outliers - some fishers are much more efficient than others.






# Basic statistical terms {#appendix-stat-terms}

Note that all the following statistical terms apply only to *numerical* variables, except the *distribution* which can exist for both numerical and categorical variables. 

## Mean

The *mean* is the most commonly reported measure of center.  It is commonly called the *average* though this term can be a little ambiguous.  The mean is the sum of all of the data elements divided by how many elements there are. If we have $n$ data points, the mean is given by: 

$$Mean = \frac{x_1 + x_2 + \cdots + x_n}{n}$$

## Median

The median is calculated by first sorting a variable's data from smallest to largest.  After sorting the data, the middle element in the list is the *median*.  If the middle falls between two values, then the median is the mean of those two middle values.

## Standard deviation

We will next discuss the *standard deviation* ($sd$) of a variable.  The formula can be a little intimidating at first but it is important to remember that it is essentially a measure of how far we expect a given data value will be from its mean:

$$sd = \sqrt{\frac{(x_1 - Mean)^2 + (x_2 - Mean)^2 + \cdots + (x_n - Mean)^2}{n - 1}}$$

## Five-number summary

The *five-number summary* consists of five summary statistics: the minimum, the first quantile AKA 25th percentile, the second quantile AKA median or 50th percentile, the third quantile AKA 75th, and the maximum. The five-number summary of a variable is used when constructing boxplots, as seen in Section \@ref(method).

The quantiles are calculated as

- first quantile ($Q_1$): the median of the first half of the sorted data
- third quantile ($Q_3$): the median of the second half of the sorted data

The *interquartile range (IQR)*\index{interquartile range (IQR)} is defined as $Q_3 - Q_1$ and is a measure of how spread out the middle 50% of values are. The IQR corresponds to the length of the box in a boxplot.

The median and the IQR are not influenced by the presence of outliers in the ways that the mean and standard deviation are. They are, thus, recommended for skewed datasets. We say in this case that the median and IQR are more *robust to outliers*.

## Distribution

The *distribution* of a variable shows how frequently different values of a variable occur. Looking at the visualization of a distribution can show where the values are centered, show how the values vary, and give some information about where a typical value might fall.  It can also alert you to the presence of outliers. 

Recall from Chapter \@ref(intro) that we can visualize the distribution of a numerical variable using binning in a histogram and that we can visualize the distribution of a categorical variable using a barplot.


## Outliers

*Outliers* correspond to values in the dataset that fall far outside the range of "ordinary" values.  In the context of a boxplot, by default they correspond to values below $Q_1 - (1.5 \cdot IQR)$ or above $Q_3 + (1.5 \cdot IQR)$.




## Normal distribution {#appendix-normal-curve}

Let's next discuss one particular kind of distribution: \index{distribution!normal} *normal distributions*. Such bell-shaped distributions are defined by two values: (1) the *mean* $\mu$ ("mu") which locates the center of the distribution and (2) the *standard deviation* $\sigma$ ("sigma") which determines the variation of the distribution. In Figure \@ref(fig:normal-curves), we plot three normal distributions where:

1. The solid normal curve has mean $\mu = 5$ \& standard deviation $\sigma = 2$.
1. The dotted normal curve has mean $\mu = 5$ \& standard deviation $\sigma = 5$.
1. The dashed normal curve has mean $\mu = 15$ \& standard deviation $\sigma = 2$.

\begin{figure}
\includegraphics[width=0.9\linewidth]{07-appendix_files/figure-latex/normal-curves-1} \caption{Three normal distributions.}(\#fig:normal-curves)
\end{figure}

Notice how the solid and dotted line normal curves have the same center due to their common mean $\mu$ = 5. However, the dotted line normal curve is wider due to its larger standard deviation of $\sigma$ = 5. On the other hand, the solid and dashed line normal curves have the same variation due to their common standard deviation $\sigma$ = 2. However, they are centered at different locations. 

When the mean $\mu$ = 0 and the standard deviation $\sigma$ = 1, the normal distribution has a special name. It's called the *standard normal distribution* or the *$z$-curve*\index{distribution!standard normal}.

Furthermore, if a variable follows a normal curve, there are *three rules of thumb* we can use:

1. 68% of values will lie within $\pm$ 1 standard deviation of the mean.
1. 95% of values will lie within $\pm$ 1.96 $\approx$ 2 standard deviations of the mean.
1. 99.7% of values will lie within $\pm$ 3 standard deviations of the mean.

Let's illustrate this on a standard normal curve in Figure \@ref(fig:normal-rule-of-thumb). The dashed lines are at -3, -1.96, -1, 0, 1, 1.96, and 3. These 7 lines cut up the x-axis into 8 segments. The areas under the normal curve for each of the 8 segments are marked and add up to 100%. For example:

1. The middle two segments represent the interval -1 to 1. The shaded area above this interval represents 34% + 34% = 68% of the area under the curve. In other words, 68% of values. 
1. The middle four segments represent the interval -1.96 to 1.96. The shaded area above this interval represents 13.5% + 34% + 34% + 13.5% = 95% of the area under the curve. In other words, 95% of values. 
1. The middle six segments represent the interval -3 to 3. The shaded area above this interval represents 2.35% + 13.5% + 34% + 34% + 13.5% + 2.35% = 99.7% of the area under the curve. In other words, 99.7% of values. 

\begin{figure}
\includegraphics[width=0.8\linewidth]{07-appendix_files/figure-latex/normal-rule-of-thumb-1} \caption{Rules of thumb about areas under normal curves.}(\#fig:normal-rule-of-thumb)
\end{figure}


## log10 transformations {#appendix-log10-transformations}

At its simplest, log10 transformations return base 10 *logarithms*. For example, since $1000 = 10^3$, running `log10(1000)` returns `3` in R. To undo a log10 transformation, we raise 10 to this value. For example, to undo the previous log10 transformation and return the original value of 1000, we raise 10 to the power of 3 by running `10^(3) = 1000` in R. \index{log transformations}. Log transformations allow us to focus on changes in *orders of magnitude*. In other words, they allow us to focus on *multiplicative changes* instead of *additive ones*. 

<!-- Let's illustrate this idea in Table \@ref(tab:logten) with examples of prices of consumer goods in 2019 US dollars.  -->

<!--
We can also frame such changes as being relative percentage increases/decreases instead of absolute increases/decreases. 
-->

<!-- ```{r logten, echo=FALSE} -->
<!-- tibble(Price = c(1,10,100,1000,10000,100000,1000000)) %>%  -->
<!--   mutate( -->
<!--     `log10(Price)` = log10(Price), -->
<!--     Price = dollar(Price), -->
<!--     `Order of magnitude` = c("Singles", "Tens", "Hundreds", "Thousands", "Tens of thousands", "Hundreds of thousands", "Millions"), -->
<!--     `Examples` = c("Cups of coffee", "Books", "Mobile phones", "High definition TVs", "Cars", "Luxury cars and houses", "Luxury houses") -->
<!--   ) %>%  -->
<!--   knitr::kable( -->
<!--     digits = 3,  -->
<!--     caption = "log10 transformed prices, orders of magnitude, and examples",  -->
<!--     booktabs = TRUE, -->
<!--     linesep = "" -->
<!--   ) %>%  -->
<!--   kable_styling(font_size = ifelse(knitr:::is_latex_output(), 10, 16),  -->
<!--                 latex_options = c("hold_position")) -->
<!-- ``` -->

<!-- Let's make some remarks about log10 transformations based on Table \@ref(tab:logten): -->

<!-- 1. When purchasing a cup of coffee, we tend to think of prices ranging in single dollars, such as \$2 or \$3. However, when purchasing a mobile phone, we don't tend to think of their prices in units of single dollars such as \$313 or \$727. Instead, we tend to think of their prices in units of hundreds of dollars like \$300 or \$700. Thus, cups of coffee and mobile phones are of different *orders of magnitude* in price. -->
<!-- 1. Let's say we want to know the log10 transformed value of \$76. This would be hard to compute exactly without a calculator. However, since \$76 is between \$10 and \$100 and since log10(10) = 1 and log10(100) = 2, we know log10(76) will be between 1 and 2. In fact, log10(76) is 1.880814. -->
<!-- 1. log10 transformations are *monotonic*, meaning they preserve orders. So if Price A is lower than Price B, then log10(Price A) will also be lower than log10(Price B). -->
<!-- 1. Most importantly, increments of one in log10-scale correspond to *relative multiplicative changes* in the original scale and not *absolute additive changes*. For example, increasing a log10(Price) from 3 to 4 corresponds to a multiplicative increase by a factor of 10: \$100 to \$1000. -->


<!-- \newpage -->
<!-- ## Appendix: all codes for this Manuscript -->
<!-- The appendix contain R programming codes used to read, manipulate, visualize, plotting and generating this manuscript in PDF format. Please ignore this section if you find the syntax too boring. But its this section is like the car engine, it power the document! -->


<!-- ```{r ref.label=knitr::all_labels(), echo = T, eval = F} -->

<!-- ``` -->

