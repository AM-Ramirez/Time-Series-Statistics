# Chapter 1 Homework
Alex Ramirez
2023-08-26

- [Book Problems](#book-problems)
  - [1.1 Annual Sunspot Data](#annual-sunspot-data)
  - [1.2 Monthly Sunspot Data](#monthly-sunspot-data)
  - [1.3 Air Passengers](#air-passengers)
  - [1.4 Total Vehicle Sales](#total-vehicle-sales)
  - [1.5 New Houses Sold b/w 1965 -
    2020](#new-houses-sold-bw-1965---2020)
  - [1.6 Kaggle USA Monthly Sales](#kaggle-usa-monthly-sales)
- [Job Finding](#job-finding)

# Book Problems

## 1.1 Annual Sunspot Data

``` r
library(tswge)
ss=read.csv("/Users/alejandroramirez/Desktop/Time Series/SN_y_tot_V2.0.csv",";",header=FALSE)
sunspot=ts(ss$V2,start=1700,frequency=1) 
sunspot_49_14 = window(sunspot,start=c(1749),end = c(1914))
data("sunspot2.0")
sunspot2.0_49_14=window(sunspot2.0,start=c(1749),end = c(1914))
plotts.wge(sunspot2.0_49_14, ylab="Sun Spots",xlab="1749 - 1914",main="Sunspot2.0 Data")
```

![](README_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
plotts.wge(sunspot_49_14,ylab="Sun Spots",xlab="1749 - 1914",main="Sunspot Classic Data", col="red")
```

![](README_files/figure-commonmark/unnamed-chunk-1-2.png)

## 1.2 Monthly Sunspot Data

``` r
data("sunspot2.0.month")
sunspot2.0.month_49_14 = window(sunspot2.0.month,start=c(1749,1),end=c(1914,12))
plotts.wge(sunspot2.0.month_49_14, ylab="Sun Spots",xlab="1749 - 1914",main="Sunspot2.0 Monthly Data")
```

![](README_files/figure-commonmark/unnamed-chunk-2-1.png)

``` r
ssm=read.csv("/Users/alejandroramirez/Desktop/Time Series/SN_m_tot_V2.0.csv",";",header=FALSE)
sunspotm = ts(ssm$V4,start=c(1749,1), frequency=12)
sunspotm_49_14 = window(sunspotm,start=c(1749,1),end=c(1914,12))
plotts.wge(sunspotm_49_14, ylab="Sun Spots",xlab="1749 - 1914",main="Sunspot2.0 Monthly  Classic Data", col="red")
```

![](README_files/figure-commonmark/unnamed-chunk-2-2.png)

## 1.3 Air Passengers

``` r
library(readxl)
airpassenger_data = read_excel("/Users/alejandroramirez/Desktop/Time Series/AirPassengers.xlsx", sheet="Data")

library(dplyr)
airpassenger_data = airpassenger_data %>% filter(Month != "TOTAL") 
head(airpassenger_data)
```

    # A tibble: 6 × 5
      Year  Month DOMESTIC INTERNATIONAL    TOTAL
      <chr> <chr>    <dbl>         <dbl>    <dbl>
    1 2002  10    48054917       9578435 57633352
    2 2002  11    44850246       9016535 53866781
    3 2002  12    49684353      10038794 59723147
    4 2003  1     43032450       9726436 52758886
    5 2003  2     41166780       8283372 49450152
    6 2003  3     49992700       9538653 59531353

``` r
airpassenger_data_new = airpassenger_data %>% select(c(DOMESTIC)) 
head(airpassenger_data_new)
```

    # A tibble: 6 × 1
      DOMESTIC
         <dbl>
    1 48054917
    2 44850246
    3 49684353
    4 43032450
    5 41166780
    6 49992700

``` r
airpassenger_data_new$DOMESTIC = airpassenger_data_new$DOMESTIC/10^6
ap_data.ts = ts(airpassenger_data_new,start=c(2002,10),frequency=12)
plotts.wge(ap_data.ts, xlab="Oct'02 through May'23", ylab = "# of Passengers in Millions", main="Domestic US Air Passengers TS", col="blue")
```

![](README_files/figure-commonmark/unnamed-chunk-3-1.png)

## 1.4 Total Vehicle Sales

1)  Download csv [URL](https://fred.stlouisfed.org/series/TOTALSA)
2)  read in csv file

``` r
# NSA.read = read.csv(file.choose(),header=TRUE)
NSA.read = read.csv("/Users/alejandroramirez/Desktop/Time Series/TOTALNSA.CSV",header=TRUE)
x = NSA.read$TOTALNSA
```

3)  Use head(x)

``` r
head(x)
```

    [1]  885.2  994.7 1243.6 1191.2 1203.2 1254.7

4)  Plot using plotts.wge(x)

``` r
plotts.wge(x,xlab="Jan 1st 1976 through Jul 1st 2021", ylab="# of Vehicles Sold", main="FRED: Total Vehicles Sales")
```

![](README_files/figure-commonmark/unnamed-chunk-6-1.png)

5)  Convert vector x to a ts file named NSA

``` r
NSA_NEW = ts(x,start=c(1776,1),frequency=12)
```

6)  Plot NSA using plotts.wge

``` r
plotts.wge(NSA_NEW,xlab="Jan 1st 1976 through Jul 1st 2021", ylab="# of Vehicles Sold", main="FRED: Total Vehicles Sales TS",col="blue")
```

![](README_files/figure-commonmark/unnamed-chunk-8-1.png)

7)  Describe the behavior of the data (cyclic, seasonal, wandering)

The data appears to have some cyclic pattern of ups and downs
(pseudo-sinusoidal), but the seasonality is hard to determine. Looking
at the data in excel, the max and the min points are typically spread 12
months apart on average.

| Months b/w Max | Months b/w Min |
|----------------|----------------|
| avg: 12.11     | avg: 12.18     |
| min: 3         | min: 1.03      |
| max: 19.3      | max: 23.33     |

## 1.5 New Houses Sold b/w 1965 - 2020

1)  download csv [URL](https://fred.stlouisfed.org/series/HSN1F)

2)  create vector x

``` r
#HSN1F.read = read.csv(file.choose(),header=TRUE)
HSN1F.read = read.csv("/Users/alejandroramirez/Desktop/Time Series/HSN1F.csv",header=TRUE)
x = HSN1F.read$HSN1F
```

3)  plot data in x using plotts.wge(x)

``` r
plotts.wge(x, xlab="Jan'65 through Dec'20", ylab="# of Homes Sold", main="FRED: Total One Family Home Sales")
```

![](README_files/figure-commonmark/unnamed-chunk-10-1.png)

4)  convert x to ts file

``` r
NHSN1F = ts(x,start=c(1965,1),frequency=12)
```

5)  Plot with plotts.wge

``` r
plotts.wge(NHSN1F,xlab="Jan'65 through Dec'20",ylab="# of Homes Sold",main="FRED: # of One Family Homes Sold TS",col="blue")
```

![](README_files/figure-commonmark/unnamed-chunk-12-1.png)

6)  **Describe behavior:**

This one is more difficult to see the cyclic nature of the \# of One
Family Homes Sold. This is skewed by the increasing trend from the 90s
to 2005, right before the crash in 2008. After the recession we saw an
increasing trend continue, accelerated by the COVID-19 Pandemic. The
maxes and mins in the calendar years are on average separated by 12
months, but this is not assigned to any one particular month.

| Months b/w Max | Months b/w Min |
|----------------|----------------|
| avg: 12.0      | avg: 12.1      |
| min: 2.0       | min: 1.0       |
| max: 23.0      | max: 21.1      |

## 1.6 Kaggle USA Monthly Sales

1)  **Download the
    [Data](https://www.kaggle.com/datasets/landlord/usa-monthly-retail-trade?resource=download)**

2)  **Read in x vector**

``` r
#NAICS.read = read.csv(file.choose(),header=TRUE)
NAICS.read = read.csv("/Users/alejandroramirez/Desktop/Time Series/SeriesReport-Not Seasonally Adjusted Sales - Monthly (Millions of Dollars).csv",header=TRUE)
x = NAICS.read$Value
```

3)  **Plot x**

``` r
plotts.wge(x,xlab="Jan'92 through Dec'20",ylab="Sales in Millions",main="KAGGLE: USA Monthly Sales Non-Seasonally Adjusted")
```

![](README_files/figure-commonmark/unnamed-chunk-14-1.png)

4)  **Convert x to TS file named NAICS through Jan’00 through Dec’19**

``` r
NAICS.ts = ts(x,start=c(1992,1),frequency=12)
NAICS.ts = window(NAICS.ts,start=c(2000,1),end=c(2019,12))
```

5)  **Convert NAICS to billions & plot**

``` r
NAICS.ts = NAICS.ts/1000
plotts.wge(NAICS.ts,xlab="Jan'00 through Dec'19",ylab="Sales in Billions",main="KAGGLE: USA Monthly Sales Non-Seasonally Adjusted TS",col="blue")
```

![](README_files/figure-commonmark/unnamed-chunk-16-1.png)

6)  **Describe the Behavior:**

    The data appears to have a definite increasing trend with some
    cyclic nature. It appears in every year, aside from 2009 and 2020,
    the maximum month of sales is in December (27 out of the 29 years
    had highs in December). This could be driven by the large amount of
    sales and offering for Holiday present spending. This would make
    this data seasonal, as it has consistent high points throughout the
    years.

# Job Finding

- TikTok ML Engineer - Demand forecasting
  [link](https://careers.tiktok.com/position/7243857058403502392/detail?spread=XKM9ZXE)
  - **Collect, clean, and preprocess large**-scale data sets to ensure
    data quality and suitability for forecasting purposes.

  - **Conduct exploratory data analysis to gain insights into demand
    patterns, trends, and seasonality factors** that influence
    purchasing behavior.

  - Build scalable and efficient data pipelines to automate data
    preprocessing, model training, and **prediction** processes.
