
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
library(ggplot2)
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.4.3

    ## Warning: package 'tidyr' was built under R version 4.4.3

    ## Warning: package 'readr' was built under R version 4.4.3

    ## Warning: package 'purrr' was built under R version 4.4.3

    ## Warning: package 'forcats' was built under R version 4.4.3

    ## Warning: package 'lubridate' was built under R version 4.4.3

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ lubridate 1.9.4     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.4     ✔ tidyr     1.3.1
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
av %>% 
  select(
    Name.Alias,
    starts_with("Death")
  ) %>% 
  head()
```

    ##                    Name.Alias Death1 Death2 Death3 Death4 Death5
    ## 1   Henry Jonathan "Hank" Pym    YES                            
    ## 2              Janet van Dyne    YES                            
    ## 3 Anthony Edward "Tony" Stark    YES                            
    ## 4         Robert Bruce Banner    YES                            
    ## 5                Thor Odinson    YES    YES                     
    ## 6      Richard Milhouse Jones     NO

``` r
deaths <- av %>% 
  pivot_longer(
    starts_with("Death"),
    names_to = "Time",
    values_to = "Died"
  ) %>% 
  select(
    URL, Name.Alias, Time, Died
  )
#head(deaths)
View(deaths)
```

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
?parse_number
```

    ## starting httpd help server ... done

``` r
deaths$Time <- parse_number(deaths$Time, na = c("", "NA"))
View(deaths)
```

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
#?round
#?count
#?filter
num1 <- deaths %>% group_by(Died) %>% filter(Died == "YES") %>% summarise(total = n()) %>% summarise(total_num = sum(total))
num1
```

    ## # A tibble: 1 × 1
    ##   total_num
    ##       <int>
    ## 1        89

``` r
num2 <- deaths %>% group_by(Died) %>% filter(Died != "") %>% summarise(total2 = n()) %>% summarise(total_num2 = sum(total2))
num1 / num2
```

    ##   total_num
    ## 1 0.4587629

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

Cole Flickinger’s Quote: “I counted 89 total deaths”: This is fact
checked to be true from the data given.

``` r
numOfDeaths <- deaths %>% group_by(Died) %>% filter(Died == "YES") %>% summarise(total = n()) %>% summarise(numOfDeaths = sum(total))
numOfDeaths
```

    ## # A tibble: 1 × 1
    ##   numOfDeaths
    ##         <int>
    ## 1          89

“The MVP of the Earth-616 Marvel Universe Avengers has to be Jocasta —
an android based on Janet van Dyne and built by Ultron — who has been
destroyed five times and then recovered five times.”

``` r
jocasta_data <- av %>% 
  filter(str_detect(Name.Alias, "Jocasta")) %>% 
  select(Name.Alias, starts_with("Death"), starts_with("Return"))

jocasta_deaths <- sum(jocasta_data %>% select(starts_with("Death")) == "YES")

jocasta_returns <- sum(jocasta_data %>% select(starts_with("Return")) == "YES")

tibble(
  Character = "Jocasta",
  Deaths = jocasta_deaths,
  Returns = jocasta_returns,
  Perfect_Recovery = (jocasta_deaths == jocasta_returns)
)
```

    ## # A tibble: 1 × 4
    ##   Character Deaths Returns Perfect_Recovery
    ##   <chr>      <int>   <int> <lgl>           
    ## 1 Jocasta        5       5 TRUE

From the data we can clearly see that Jocasta has died and returned 5
times.

Sofi Gutierrez Quote: “Out of 173 listed Avengers, my analysis found
that 69 had died at least one time after they joined the team”

``` r
# Count the total Avengers listed in the dataset
total_avengers <- av %>% 
  summarise(total_listed = n())

# Filter Avengers who have died at least once
avengers_with_deaths <- av %>% 
  filter(if_any(starts_with("Death"), ~ .x == "YES")) %>% 
  summarise(total_died = n())

# Display results and validate against the claim
tibble(
  Total_Listed_Avengers = total_avengers$total_listed,
  Avengers_Who_Died_At_Least_Once = avengers_with_deaths$total_died,
  Match_Claim = (total_avengers$total_listed == 173 && avengers_with_deaths$total_died == 69)
)
```

    ## # A tibble: 1 × 3
    ##   Total_Listed_Avengers Avengers_Who_Died_At_Least_Once Match_Claim
    ##                   <int>                           <int> <lgl>      
    ## 1                   173                              69 TRUE

This showing the statement is true \### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.
