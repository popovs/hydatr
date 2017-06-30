
<!-- README.md is generated from README.Rmd. Please edit that file -->
hydatr
======

The HYDAT database contains over 1 GB of hydrological observation data collected by Environment Canada. The database is freely available from the Environment Canada website, however exctracting data can be difficult. This package provides tools to easily find and extract hydrological data from the HYDAT database.

Installation
------------

You can install hydatr from github with:

``` r
# install.packages("devtools")
devtools::install_github("paleolimbot/hydatr")
```

Getting the HYDAT Database
--------------------------

The HYDAT database is about 1 GB in size, which is compressed into a 200 MB download from <http://collaboration.cmc.ec.gc.ca/cmc/hydrometrics/www/> . The hydat package will happily download and extract this form you.

``` r
library(hydatr)
hydat_download() # downloads the database from environment canada (10 min)
hydat_extract() # extracts the downloaded database into readable form
hydat_load() # loads the database (you'll need to call this one each time you load the package)
```

Finding Hydro Stations
----------------------

Find hydro sites:

``` r
hydat_find_stations("lower sackville, NS", year = 1999:2012)
#> # A tibble: 10 × 5
#>    STATION_NUMBER dist_from_query_km
#>             <chr>              <dbl>
#> 1         01EJ004          0.9861109
#> 2         01EJ001          5.1509276
#> 3         01DG003          8.8762890
#> 4         01DJ005         74.3590516
#> 5         01EF001         80.2094515
#> 6         01DC005        107.6006889
#> 7         01DP004        107.6301656
#> 8         01DL001        108.5261888
#> 9         01EE005        113.1515549
#> 10        01DC007        116.6305481
#> # ... with 3 more variables: STATION_NAME <chr>, FIRST_YEAR <int>,
#> #   LAST_YEAR <int>
```

Get detailed information about hydro sites:

``` r
as.list(hydat_station_info("01EJ004"))
#> $STATION_NUMBER
#> [1] "01EJ004"
#> 
#> $STATION_NAME
#> [1] "LITTLE SACKVILLE RIVER AT MIDDLE SACKVILLE"
#> 
#> $PROV_TERR_STATE_LOC
#> [1] "NS"
#> 
#> $LATITUDE
#> [1] 44.76447
#> 
#> $LONGITUDE
#> [1] -63.6875
#> 
#> $DRAINAGE_AREA_GROSS
#> [1] 13.1
#> 
#> $DRAINAGE_AREA_EFFECT
#> [1] NA
#> 
#> $STATUS_EN_HYD
#> [1] "Active"
#> 
#> $STATUS_EN_SED
#> [1] "Discontinued"
#> 
#> $REGIONAL_OFFICE_NAME_EN
#> [1] "DARTMOUTH"
#> 
#> $AGENCY_EN_CONTRIBUTOR
#> [1] "NOVA SCOTIA DEPARTMENT OF ENVIRONMENT"
#> 
#> $AGENCY_EN_OPERATOR
#> [1] "WATER SURVEY OF CANADA (DOE) (CANADA)"
#> 
#> $RHBN
#> [1] 0
#> 
#> $REAL_TIME
#> [1] 1
#> 
#> $DATUM_ID
#> [1] 10
#> 
#> $FIRST_YEAR
#> [1] 1980
#> 
#> $LAST_YEAR
#> [1] 2015
```

Extracting Data
---------------

The following methods extract data from the database given the correct station number:

``` r
hydat_flow_monthly("01AD001")
#> # A tibble: 880 × 9
#>    STATION_NUMBER                                      STATION_NAME  YEAR
#>             <chr>                                             <chr> <int>
#> 1         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 2         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 3         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 4         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 5         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1919
#> 6         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1919
#> 7         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1919
#> 8         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1919
#> 9         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1919
#> 10        01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1919
#> # ... with 870 more rows, and 6 more variables: MONTH <int>, DATE <date>,
#> #   MONTHLY_MEAN <dbl>, MONTHLY_TOTAL <dbl>, DAILY_MIN <dbl>,
#> #   DAILY_MAX <dbl>
hydat_flow_daily("01AD001")
#> # A tibble: 26,785 × 8
#>    STATION_NUMBER                                      STATION_NAME  YEAR
#>             <chr>                                             <chr> <int>
#> 1         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 2         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 3         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 4         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 5         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 6         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 7         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 8         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 9         01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> 10        01AD001 MADAWASKA (RIVIER) EN AVAL DU BARRAGE TEMISCOUATA  1918
#> # ... with 26,775 more rows, and 5 more variables: MONTH <int>, DAY <int>,
#> #   DATE <date>, FLOW <dbl>, FLOW_SYMBOL <chr>
hydat_level_monthly("01AD003")
#> # A tibble: 60 × 9
#>    STATION_NUMBER                                STATION_NAME  YEAR MONTH
#>             <chr>                                       <chr> <int> <int>
#> 1         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 2         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     2
#> 3         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     3
#> 4         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     4
#> 5         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     5
#> 6         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     6
#> 7         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     7
#> 8         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     8
#> 9         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     9
#> 10        01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011    10
#> # ... with 50 more rows, and 5 more variables: DATE <date>,
#> #   MONTHLY_MEAN <dbl>, MONTHLY_TOTAL <dbl>, DAILY_MIN <dbl>,
#> #   DAILY_MAX <dbl>
hydat_level_daily("01AD003")
#> # A tibble: 1,826 × 8
#>    STATION_NUMBER                                STATION_NAME  YEAR MONTH
#>             <chr>                                       <chr> <int> <int>
#> 1         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 2         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 3         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 4         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 5         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 6         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 7         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 8         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 9         01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> 10        01AD003 ST. FRANCIS RIVER AT OUTLET OF GLASIER LAKE  2011     1
#> # ... with 1,816 more rows, and 4 more variables: DAY <int>, DATE <date>,
#> #   LEVEL <dbl>, LEVEL_SYMBOL <chr>
hydat_sed_monthly("01AF006")
#> # A tibble: 26 × 9
#>    STATION_NUMBER                           STATION_NAME  YEAR MONTH
#>             <chr>                                  <chr> <int> <int>
#> 1         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4
#> 2         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     5
#> 3         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     6
#> 4         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     7
#> 5         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     8
#> 6         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     9
#> 7         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1972     4
#> 8         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1972     5
#> 9         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1972     6
#> 10        01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1972     7
#> # ... with 16 more rows, and 5 more variables: DATE <date>,
#> #   MONTHLY_MEAN <dbl>, MONTHLY_TOTAL <dbl>, DAILY_MIN <dbl>,
#> #   DAILY_MAX <dbl>
hydat_sed_daily("01AF006")
#> # A tibble: 794 × 7
#>    STATION_NUMBER                           STATION_NAME  YEAR MONTH   DAY
#>             <chr>                                  <chr> <int> <int> <int>
#> 1         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     1
#> 2         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     2
#> 3         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     3
#> 4         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     4
#> 5         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     5
#> 6         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     6
#> 7         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     7
#> 8         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     8
#> 9         01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4     9
#> 10        01AF006 BLACK BROOK NEAR ST-ANDRE-DE-MADAWASKA  1971     4    10
#> # ... with 784 more rows, and 2 more variables: DATE <date>, LOAD <dbl>
```

Feedback
--------

The `hydatr` package is in development, so feel free to send feedback to <dewey@fishandwhistle.net>