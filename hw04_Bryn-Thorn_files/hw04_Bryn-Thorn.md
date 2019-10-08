---
title: "hw04"
author: "Arlo"
output: 
  html_document:
    toc: yes
    keep_md: yes
---





# Exercise 1 (Univariate Option 1)

## 1.1: Tibble of life expectancy over time for Canada and Rwanda 

Filter gapminder data by selecting Canadian and Rwandan data, then select so that data is organized by country then year in ascending order, then lifeExp. Organize data into a tibble where each row is a unique observation (year), then shows Canadian and Rwandan life expectancy in their own columns.  

```r
gapminder %>% 
  filter(country == "Canada" | country == "Rwanda") %>%
  select(country, year, lifeExp) %>% 
  pivot_wider(id_cols        = year, 
              names_from  = country, 
              values_from = lifeExp)
```

```
## # A tibble: 12 x 3
##     year Canada Rwanda
##    <int>  <dbl>  <dbl>
##  1  1952   68.8   40  
##  2  1957   70.0   41.5
##  3  1962   71.3   43  
##  4  1967   72.1   44.1
##  5  1972   72.9   44.6
##  6  1977   74.2   45  
##  7  1982   75.8   46.2
##  8  1987   76.9   44.0
##  9  1992   78.0   23.6
## 10  1997   78.6   36.1
## 11  2002   79.8   43.4
## 12  2007   80.7   46.2
```

## 1.2: Scatterplot of Canadian and Rwandan life expectancy over time

Plot data against eachother, showing how change in life expectancy over time compares between Canada and Rwanda

```r
gapminder %>% 
  filter(country == "Canada" | country == "Rwanda") %>%
  select(country, year, lifeExp) %>% 
  pivot_wider(id_cols        = year, 
              names_from  = country, 
              values_from = lifeExp) %>% 
  ggplot(aes(Canada, Rwanda)) +
  geom_point() +
  theme_bw() +
  geom_text(aes(label=year),hjust=0, vjust=0)
```

![](hw04_Bryn-Thorn_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

## 1.3: Re-lengthening data

Relengthen data back into long format. 

```r
gapminder %>% 
  filter(country == "Canada" | country == "Rwanda") %>%
  select(country, year, lifeExp) %>% 
  pivot_wider(id_cols        = year, 
              names_from  = country, 
              values_from = lifeExp) %>%
  pivot_longer(cols = c(Canada, Rwanda), 
               names_to = "country", 
               values_to = "lifeExp") 
```

```
## # A tibble: 24 x 3
##     year country lifeExp
##    <int> <chr>     <dbl>
##  1  1952 Canada     68.8
##  2  1952 Rwanda     40  
##  3  1957 Canada     70.0
##  4  1957 Rwanda     41.5
##  5  1962 Canada     71.3
##  6  1962 Rwanda     43  
##  7  1967 Canada     72.1
##  8  1967 Rwanda     44.1
##  9  1972 Canada     72.9
## 10  1972 Rwanda     44.6
## # … with 14 more rows
```

# Exercise 2 (Multivariate Option 1)

## 2.1: Tibble showing Canadian and Rwandan lifeExp and gdpPercap per year 

Add columns showing lifeExp and gdpPercap for Canada and Rwanda (separately).

```r
gapminder %>% 
  filter(country == "Canada" | country == "Rwanda") %>%
  select(country, year, lifeExp, gdpPercap) %>% 
  pivot_wider(id_cols        = year, 
              names_from  = country,
              names_sep = "_",
              values_from = c(lifeExp, gdpPercap))
```

```
## # A tibble: 12 x 5
##     year lifeExp_Canada lifeExp_Rwanda gdpPercap_Canada gdpPercap_Rwanda
##    <int>          <dbl>          <dbl>            <dbl>            <dbl>
##  1  1952           68.8           40             11367.             493.
##  2  1957           70.0           41.5           12490.             540.
##  3  1962           71.3           43             13462.             597.
##  4  1967           72.1           44.1           16077.             511.
##  5  1972           72.9           44.6           18971.             591.
##  6  1977           74.2           45             22091.             670.
##  7  1982           75.8           46.2           22899.             882.
##  8  1987           76.9           44.0           26627.             848.
##  9  1992           78.0           23.6           26343.             737.
## 10  1997           78.6           36.1           28955.             590.
## 11  2002           79.8           43.4           33329.             786.
## 12  2007           80.7           46.2           36319.             863.
```

## 2.2: Re-lengthening data

Relengthen data back into long format. 

```r
gapminder %>% 
  filter(country == "Canada" | country == "Rwanda") %>%
  select(country, year, lifeExp, gdpPercap) %>% 
  pivot_wider(id_cols        = year, 
              names_from  = country,
              names_sep = "_",
              values_from = c(lifeExp, gdpPercap)) %>%
  pivot_longer(cols = -year,
               names_sep = "_",
               names_to = c(".value", "country"))
```

```
## # A tibble: 24 x 4
##     year country lifeExp gdpPercap
##    <int> <chr>     <dbl>     <dbl>
##  1  1952 Canada     68.8    11367.
##  2  1952 Rwanda     40        493.
##  3  1957 Canada     70.0    12490.
##  4  1957 Rwanda     41.5      540.
##  5  1962 Canada     71.3    13462.
##  6  1962 Rwanda     43        597.
##  7  1967 Canada     72.1    16077.
##  8  1967 Rwanda     44.1      511.
##  9  1972 Canada     72.9    18971.
## 10  1972 Rwanda     44.6      591.
## # … with 14 more rows
```


# Exercise 3

Gather data from online repository.

```r
guest <- read_csv("https://raw.githubusercontent.com/STAT545-UBC/Classroom/master/data/wedding/attend.csv")
```

```
## Parsed with column specification:
## cols(
##   party = col_integer(),
##   name = col_character(),
##   meal_wedding = col_character(),
##   meal_brunch = col_character(),
##   attendance_wedding = col_character(),
##   attendance_brunch = col_character(),
##   attendance_golf = col_character()
## )
```

```r
email <- read_csv("https://raw.githubusercontent.com/STAT545-UBC/Classroom/master/data/wedding/emails.csv")
```

```
## Parsed with column specification:
## cols(
##   guest = col_character(),
##   email = col_character()
## )
```

## 3.0: Visualize data

Print out unfamiliar datasets to visualize for best analysis.

```r
guest
```

```
## # A tibble: 30 x 7
##    party name  meal_wedding meal_brunch attendance_wedd… attendance_brun…
##    <int> <chr> <chr>        <chr>       <chr>            <chr>           
##  1     1 Somm… PENDING      PENDING     PENDING          PENDING         
##  2     1 Phil… vegetarian   Menu C      CONFIRMED        CONFIRMED       
##  3     1 Blan… chicken      Menu A      CONFIRMED        CONFIRMED       
##  4     1 Emaa… PENDING      PENDING     PENDING          PENDING         
##  5     2 Blai… chicken      Menu C      CONFIRMED        CONFIRMED       
##  6     2 Nige… <NA>         <NA>        CANCELLED        CANCELLED       
##  7     3 Sine… PENDING      PENDING     PENDING          PENDING         
##  8     4 Ayra… vegetarian   Menu B      PENDING          PENDING         
##  9     5 Atla… PENDING      PENDING     PENDING          PENDING         
## 10     5 Denz… fish         Menu B      CONFIRMED        CONFIRMED       
## # … with 20 more rows, and 1 more variable: attendance_golf <chr>
```

```r
email
```

```
## # A tibble: 14 x 2
##    guest                                             email                 
##    <chr>                                             <chr>                 
##  1 Sommer Medrano, Phillip Medrano, Blanka Medrano,… sommm@gmail.com       
##  2 Blair Park, Nigel Webb                            bpark@gmail.com       
##  3 Sinead English                                    singlish@hotmail.ca   
##  4 Ayra Marks                                        marksa42@gmail.com    
##  5 Jolene Welsh, Hayley Booker                       jw1987@hotmail.com    
##  6 Amayah Sanford, Erika Foley                       erikaaaaaa@gmail.com  
##  7 Ciaron Acosta                                     shining_ciaron@gmail.…
##  8 Diana Stuart                                      doodledianastu@gmail.…
##  9 Daisy-May Caldwell, Martin Caldwell, Violet Cald… caldwellfamily5212@gm…
## 10 Rosanna Bird, Kurtis Frost                        rosy1987b@gmail.com   
## 11 Huma Stokes, Samuel Rutledge                      humastokes@gmail.com  
## 12 Eddison Collier, Stewart Nicholls                 eddison.collier@gmail…
## 13 Turner Jones                                      tjjones12@hotmail.ca  
## 14 Albert Marshall, Vivian Marshall                  themarshallfamily1234…
```

## 3.1-1: Put guests in "email" data unique rows to then add to "guestlist" data

Define new variable that will hold the newly separated list of emails.

```r
email_separate <- email %>% 
  separate_rows(guest, sep = ", ")
```
##3.1-2: Add column for email to guestlist data

Join new dataset held in new variable to "guest" dataset.

```r
guest %>%
  left_join(email_separate, c("name" = "guest")) 
```

```
## # A tibble: 30 x 8
##    party name  meal_wedding meal_brunch attendance_wedd… attendance_brun…
##    <int> <chr> <chr>        <chr>       <chr>            <chr>           
##  1     1 Somm… PENDING      PENDING     PENDING          PENDING         
##  2     1 Phil… vegetarian   Menu C      CONFIRMED        CONFIRMED       
##  3     1 Blan… chicken      Menu A      CONFIRMED        CONFIRMED       
##  4     1 Emaa… PENDING      PENDING     PENDING          PENDING         
##  5     2 Blai… chicken      Menu C      CONFIRMED        CONFIRMED       
##  6     2 Nige… <NA>         <NA>        CANCELLED        CANCELLED       
##  7     3 Sine… PENDING      PENDING     PENDING          PENDING         
##  8     4 Ayra… vegetarian   Menu B      PENDING          PENDING         
##  9     5 Atla… PENDING      PENDING     PENDING          PENDING         
## 10     5 Denz… fish         Menu B      CONFIRMED        CONFIRMED       
## # … with 20 more rows, and 2 more variables: attendance_golf <chr>,
## #   email <chr>
```


## 3.2: Non-guestlist emails identified

Find out who has email data that is not on the guestlist.

```r
email_separate %>% 
  anti_join(guest, c("guest" = "name"))
```

```
## # A tibble: 3 x 2
##   guest           email                          
##   <chr>           <chr>                          
## 1 Turner Jones    tjjones12@hotmail.ca           
## 2 Albert Marshall themarshallfamily1234@gmail.com
## 3 Vivian Marshall themarshallfamily1234@gmail.com
```

## 3.3: Create full guestlist

Join data for all guests whether or not the have an email associated with them (using full_join() allows for "N/A" observations to be included)

```r
guest %>% 
  full_join(email_separate, c("name" = "guest")) 
```

```
## # A tibble: 33 x 8
##    party name  meal_wedding meal_brunch attendance_wedd… attendance_brun…
##    <int> <chr> <chr>        <chr>       <chr>            <chr>           
##  1     1 Somm… PENDING      PENDING     PENDING          PENDING         
##  2     1 Phil… vegetarian   Menu C      CONFIRMED        CONFIRMED       
##  3     1 Blan… chicken      Menu A      CONFIRMED        CONFIRMED       
##  4     1 Emaa… PENDING      PENDING     PENDING          PENDING         
##  5     2 Blai… chicken      Menu C      CONFIRMED        CONFIRMED       
##  6     2 Nige… <NA>         <NA>        CANCELLED        CANCELLED       
##  7     3 Sine… PENDING      PENDING     PENDING          PENDING         
##  8     4 Ayra… vegetarian   Menu B      PENDING          PENDING         
##  9     5 Atla… PENDING      PENDING     PENDING          PENDING         
## 10     5 Denz… fish         Menu B      CONFIRMED        CONFIRMED       
## # … with 23 more rows, and 2 more variables: attendance_golf <chr>,
## #   email <chr>
```
