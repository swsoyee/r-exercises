Beyond the basics of data.table: Smooth data exploration
================
sindri
6 September 2017

![](https://www.r-exercises.com/wp-content/uploads/2017/08/33232402401_03b48d3268_m.jpg)

This exercise set provides practice using the fast and concise
data.table package. If you are new to the syntax it is recommended that
you start by solving [the set on the basics of
data.table](http://www.r-exercises.com/2017/08/23/basics-of-data-table-smooth-data-exploration/)
before attempting this one.

We will use data on used cars (Toyota Corollas) on sale during 2004 in
the Netherlands. There are 1436 observations with information on the
price at which it is offered for sale, age, mileage and more, see full
variable description
[here](http://www.r-exercises.com/2017/08/31/data-description-for-exercise-beyond-the-basics-of-data-table-smooth-data-exploration).

Answers are available
[here](http://www.r-exercises.com/2017/09/06/beyond-the-basics-of-data-table-smooth-data-exploration-solutions).

## Exercise 1

Load the
[data](http://www.r-exercises.com/wp-content/uploads/2017/08/toy_cor.csv)
available to your working environment using `fread()`, don’t forget to
load the `data.table` package first.

``` r
library(data.table)

df <- fread("http://www.r-exercises.com/wp-content/uploads/2017/08/toy_cor.csv")
df
```

    ##                                      Model Price Age_08_04 Mfg_Month
    ##    1:       2.0 D4D HATCHB TERRA 2/3-Doors 13500        23        10
    ##    2:       2.0 D4D HATCHB TERRA 2/3-Doors 13750        23        10
    ##    3:       2.0 D4D HATCHB TERRA 2/3-Doors 13950        24         9
    ##    4:       2.0 D4D HATCHB TERRA 2/3-Doors 14950        26         7
    ##    5:         2.0 D4D HATCHB SOL 2/3-Doors 13750        30         3
    ##   ---                                                               
    ## 1432:          1.3 16V HATCHB G6 2/3-Doors  7500        69        12
    ## 1433: 1.3 16V HATCHB LINEA TERRA 2/3-Doors 10845        72         9
    ## 1434: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  8500        71        10
    ## 1435: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  7250        70        11
    ## 1436:         1.6 LB LINEA TERRA 4/5-Doors  6950        76         5
    ##       Mfg_Year    KM Fuel_Type  HP Met_Color  Color Automatic   CC Doors
    ##    1:     2002 46986    Diesel  90         1   Blue         0 2000     3
    ##    2:     2002 72937    Diesel  90         1 Silver         0 2000     3
    ##    3:     2002 41711    Diesel  90         1   Blue         0 2000     3
    ##    4:     2002 48000    Diesel  90         0  Black         0 2000     3
    ##    5:     2002 38500    Diesel  90         0  Black         0 2000     3
    ##   ---                                                                   
    ## 1432:     1998 20544    Petrol  86         1   Blue         0 1300     3
    ## 1433:     1998 19000    Petrol  86         0   Grey         0 1300     3
    ## 1434:     1998 17016    Petrol  86         0   Blue         0 1300     3
    ## 1435:     1998 16916    Petrol  86         1   Grey         0 1300     3
    ## 1436:     1998     1    Petrol 110         0  Green         0 1600     5
    ##       Cylinders Gears Quarterly_Tax Weight Mfr_Guarantee BOVAG_Guarantee
    ##    1:         4     5           210   1165             0               1
    ##    2:         4     5           210   1165             0               1
    ##    3:         4     5           210   1165             1               1
    ##    4:         4     5           210   1165             1               1
    ##    5:         4     5           210   1170             1               1
    ##   ---                                                                   
    ## 1432:         4     5            69   1025             1               1
    ## 1433:         4     5            69   1015             0               1
    ## 1434:         4     5            69   1015             0               1
    ## 1435:         4     5            69   1015             1               1
    ## 1436:         4     5            19   1114             0               0
    ##       Guarantee_Period ABS Airbag_1 Airbag_2 Airco Automatic_airco
    ##    1:                3   1        1        1     0               0
    ##    2:                3   1        1        1     1               0
    ##    3:                3   1        1        1     0               0
    ##    4:                3   1        1        1     0               0
    ##    5:                3   1        1        1     1               0
    ##   ---                                                             
    ## 1432:                3   1        1        1     1               0
    ## 1433:                3   1        1        1     0               0
    ## 1434:                3   0        1        1     0               0
    ## 1435:                3   0        0        0     0               0
    ## 1436:                3   0        1        0     0               0
    ##       Boardcomputer CD_Player Central_Lock Powered_Windows Power_Steering
    ##    1:             1         0            1               1              1
    ##    2:             1         1            1               0              1
    ##    3:             1         0            0               0              1
    ##    4:             1         0            0               0              1
    ##    5:             1         0            1               1              1
    ##   ---                                                                    
    ## 1432:             0         0            1               1              1
    ## 1433:             0         0            0               0              1
    ## 1434:             0         0            0               0              1
    ## 1435:             0         0            0               0              0
    ## 1436:             0         0            0               0              1
    ##       Radio Mistlamps Sport_Model Backseat_Divider Metallic_Rim
    ##    1:     0         0           0                1            0
    ##    2:     0         0           0                1            0
    ##    3:     0         0           0                1            0
    ##    4:     0         0           0                1            0
    ##    5:     0         1           0                1            0
    ##   ---                                                          
    ## 1432:     0         1           1                1            0
    ## 1433:     0         0           1                1            0
    ## 1434:     0         0           0                1            0
    ## 1435:     0         0           0                1            0
    ## 1436:     0         0           0                0            0
    ##       Radio_cassette Parking_Assistant Tow_Bar
    ##    1:              0                 0       0
    ##    2:              0                 0       0
    ##    3:              0                 0       0
    ##    4:              0                 0       0
    ##    5:              0                 0       0
    ##   ---                                         
    ## 1432:              0                 0       0
    ## 1433:              0                 0       0
    ## 1434:              0                 0       0
    ## 1435:              0                 0       0
    ## 1436:              0                 0       0

## Exercise 2

Using one line of code print out the most common car model in the data,
and the number of times it appears.

``` r
df[, .N, by = Model][order(-N)][1]
```

    ##                                   Model   N
    ## 1: 1.6 16V HATCHB LINEA TERRA 2/3-Doors 109

## Exercise 3

Print out the mean and median price of the 10 most common models.

``` r
df[, .(.N, 
       meanPrice = mean(Price), 
       medianPrice = median(Price)
       ), by = Model][order(-N)][1:10]
```

    ##                                      Model   N meanPrice medianPrice
    ##  1:   1.6 16V HATCHB LINEA TERRA 2/3-Doors 109  8578.440        8750
    ##  2:   1.3 16V HATCHB LINEA TERRA 2/3-Doors  84  8079.167        7950
    ##  3:     1.6 16V LIFTB LINEA LUNA 4/5-Doors  80  9454.312        9500
    ##  4:    1.6 16V LIFTB LINEA TERRA 4/5-Doors  71  8624.775        8750
    ##  5:   1.4 16V VVT I HATCHB TERRA 2/3-Doors  54 10448.704       10475
    ##  6:    1.6 16V SEDAN LINEA TERRA 4/5-Doors  43  8309.884        8250
    ##  7:    1.6 16V VVT I LIFTB TERRA 4/5-Doors  37 11514.324       11695
    ##  8:      1.6 16V VVT I LIFTB SOL 4/5-Doors  35 12131.143       11950
    ##  9:    1.3 16V LIFTB LINEA TERRA 4/5-Doors  35  8409.143        8250
    ## 10: 1.6 16V WAGON LINEA TERRA Stationwagen  28  9538.929        9225

## Exercise 4

Delete all columns that have `Guarantee` in its name.

``` r
names(df)
```

    ##  [1] "Model"             "Price"             "Age_08_04"        
    ##  [4] "Mfg_Month"         "Mfg_Year"          "KM"               
    ##  [7] "Fuel_Type"         "HP"                "Met_Color"        
    ## [10] "Color"             "Automatic"         "CC"               
    ## [13] "Doors"             "Cylinders"         "Gears"            
    ## [16] "Quarterly_Tax"     "Weight"            "Mfr_Guarantee"    
    ## [19] "BOVAG_Guarantee"   "Guarantee_Period"  "ABS"              
    ## [22] "Airbag_1"          "Airbag_2"          "Airco"            
    ## [25] "Automatic_airco"   "Boardcomputer"     "CD_Player"        
    ## [28] "Central_Lock"      "Powered_Windows"   "Power_Steering"   
    ## [31] "Radio"             "Mistlamps"         "Sport_Model"      
    ## [34] "Backseat_Divider"  "Metallic_Rim"      "Radio_cassette"   
    ## [37] "Parking_Assistant" "Tow_Bar"

``` r
df[, grep(pattern = "Guarantee", names(df)) := NULL]
names(df)
```

    ##  [1] "Model"             "Price"             "Age_08_04"        
    ##  [4] "Mfg_Month"         "Mfg_Year"          "KM"               
    ##  [7] "Fuel_Type"         "HP"                "Met_Color"        
    ## [10] "Color"             "Automatic"         "CC"               
    ## [13] "Doors"             "Cylinders"         "Gears"            
    ## [16] "Quarterly_Tax"     "Weight"            "ABS"              
    ## [19] "Airbag_1"          "Airbag_2"          "Airco"            
    ## [22] "Automatic_airco"   "Boardcomputer"     "CD_Player"        
    ## [25] "Central_Lock"      "Powered_Windows"   "Power_Steering"   
    ## [28] "Radio"             "Mistlamps"         "Sport_Model"      
    ## [31] "Backseat_Divider"  "Metallic_Rim"      "Radio_cassette"   
    ## [34] "Parking_Assistant" "Tow_Bar"

## Exercise 5

Add a new column which is the squared deviation of `price` from the
average `price` of cars the same `color`.

``` r
df[, sq_dev_by_colorl := (Price - mean(Price))^2,  by = Color]
df
```

    ##                                      Model Price Age_08_04 Mfg_Month
    ##    1:       2.0 D4D HATCHB TERRA 2/3-Doors 13500        23        10
    ##    2:       2.0 D4D HATCHB TERRA 2/3-Doors 13750        23        10
    ##    3:       2.0 D4D HATCHB TERRA 2/3-Doors 13950        24         9
    ##    4:       2.0 D4D HATCHB TERRA 2/3-Doors 14950        26         7
    ##    5:         2.0 D4D HATCHB SOL 2/3-Doors 13750        30         3
    ##   ---                                                               
    ## 1432:          1.3 16V HATCHB G6 2/3-Doors  7500        69        12
    ## 1433: 1.3 16V HATCHB LINEA TERRA 2/3-Doors 10845        72         9
    ## 1434: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  8500        71        10
    ## 1435: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  7250        70        11
    ## 1436:         1.6 LB LINEA TERRA 4/5-Doors  6950        76         5
    ##       Mfg_Year    KM Fuel_Type  HP Met_Color  Color Automatic   CC Doors
    ##    1:     2002 46986    Diesel  90         1   Blue         0 2000     3
    ##    2:     2002 72937    Diesel  90         1 Silver         0 2000     3
    ##    3:     2002 41711    Diesel  90         1   Blue         0 2000     3
    ##    4:     2002 48000    Diesel  90         0  Black         0 2000     3
    ##    5:     2002 38500    Diesel  90         0  Black         0 2000     3
    ##   ---                                                                   
    ## 1432:     1998 20544    Petrol  86         1   Blue         0 1300     3
    ## 1433:     1998 19000    Petrol  86         0   Grey         0 1300     3
    ## 1434:     1998 17016    Petrol  86         0   Blue         0 1300     3
    ## 1435:     1998 16916    Petrol  86         1   Grey         0 1300     3
    ## 1436:     1998     1    Petrol 110         0  Green         0 1600     5
    ##       Cylinders Gears Quarterly_Tax Weight ABS Airbag_1 Airbag_2 Airco
    ##    1:         4     5           210   1165   1        1        1     0
    ##    2:         4     5           210   1165   1        1        1     1
    ##    3:         4     5           210   1165   1        1        1     0
    ##    4:         4     5           210   1165   1        1        1     0
    ##    5:         4     5           210   1170   1        1        1     1
    ##   ---                                                                 
    ## 1432:         4     5            69   1025   1        1        1     1
    ## 1433:         4     5            69   1015   1        1        1     0
    ## 1434:         4     5            69   1015   0        1        1     0
    ## 1435:         4     5            69   1015   0        0        0     0
    ## 1436:         4     5            19   1114   0        1        0     0
    ##       Automatic_airco Boardcomputer CD_Player Central_Lock Powered_Windows
    ##    1:               0             1         0            1               1
    ##    2:               0             1         1            1               0
    ##    3:               0             1         0            0               0
    ##    4:               0             1         0            0               0
    ##    5:               0             1         0            1               1
    ##   ---                                                                     
    ## 1432:               0             0         0            1               1
    ## 1433:               0             0         0            0               0
    ## 1434:               0             0         0            0               0
    ## 1435:               0             0         0            0               0
    ## 1436:               0             0         0            0               0
    ##       Power_Steering Radio Mistlamps Sport_Model Backseat_Divider
    ##    1:              1     0         0           0                1
    ##    2:              1     0         0           0                1
    ##    3:              1     0         0           0                1
    ##    4:              1     0         0           0                1
    ##    5:              1     0         1           0                1
    ##   ---                                                            
    ## 1432:              1     0         1           1                1
    ## 1433:              1     0         0           1                1
    ## 1434:              1     0         0           0                1
    ## 1435:              0     0         0           0                1
    ## 1436:              1     0         0           0                0
    ##       Metallic_Rim Radio_cassette Parking_Assistant Tow_Bar
    ##    1:            0              0                 0       0
    ##    2:            0              0                 0       0
    ##    3:            0              0                 0       0
    ##    4:            0              0                 0       0
    ##    5:            0              0                 0       0
    ##   ---                                                      
    ## 1432:            0              0                 0       0
    ## 1433:            0              0                 0       0
    ## 1434:            0              0                 0       0
    ## 1435:            0              0                 0       0
    ## 1436:            0              0                 0       0
    ##       sq_dev_by_colorl
    ##    1:          7094562
    ##    2:          7178754
    ##    3:          9694267
    ##    4:         15179999
    ##    5:          7269235
    ##   ---                 
    ## 1432:         11131820
    ## 1433:          1171465
    ## 1434:          5458943
    ## 1435:         21877530
    ## 1436:          8329626

## Exercise 6

Use a combintation of `.SDcols` and `lapply` to get the `mean` value of
columns `18` through
    `35`.

``` r
df[, lapply(.SD, mean), .SDcols = 18:35]
```

    ##          ABS  Airbag_1  Airbag_2     Airco Automatic_airco Boardcomputer
    ## 1: 0.8133705 0.9707521 0.7228412 0.5083565      0.05640669     0.2945682
    ##    CD_Player Central_Lock Powered_Windows Power_Steering     Radio
    ## 1:  0.218663    0.5800836       0.5619777      0.9777159 0.1462396
    ##    Mistlamps Sport_Model Backseat_Divider Metallic_Rim Radio_cassette
    ## 1: 0.2569638   0.3001393         0.770195    0.2047354      0.1455432
    ##    Parking_Assistant   Tow_Bar
    ## 1:       0.002785515 0.2778552

## Exercise 7

Print the most common `color` by `age` in
years?

``` r
df[, (.N), by = .(Years = floor(Age_08_04/12), Color)][order(Years, V1), .SD[.N], by = Years]
```

    ##    Years Color  V1
    ## 1:     0  Grey  18
    ## 2:     1  Blue  29
    ## 3:     2  Grey  37
    ## 4:     3   Red  39
    ## 5:     4   Red  60
    ## 6:     5   Red 103
    ## 7:     6  Blue  71

## Exercise 8

For the dummy variables in columns `18:35` recode `0` to `-1`. You might
want to use the `set` function
    here.

``` r
df
```

    ##                                      Model Price Age_08_04 Mfg_Month
    ##    1:       2.0 D4D HATCHB TERRA 2/3-Doors 13500        23        10
    ##    2:       2.0 D4D HATCHB TERRA 2/3-Doors 13750        23        10
    ##    3:       2.0 D4D HATCHB TERRA 2/3-Doors 13950        24         9
    ##    4:       2.0 D4D HATCHB TERRA 2/3-Doors 14950        26         7
    ##    5:         2.0 D4D HATCHB SOL 2/3-Doors 13750        30         3
    ##   ---                                                               
    ## 1432:          1.3 16V HATCHB G6 2/3-Doors  7500        69        12
    ## 1433: 1.3 16V HATCHB LINEA TERRA 2/3-Doors 10845        72         9
    ## 1434: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  8500        71        10
    ## 1435: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  7250        70        11
    ## 1436:         1.6 LB LINEA TERRA 4/5-Doors  6950        76         5
    ##       Mfg_Year    KM Fuel_Type  HP Met_Color  Color Automatic   CC Doors
    ##    1:     2002 46986    Diesel  90         1   Blue         0 2000     3
    ##    2:     2002 72937    Diesel  90         1 Silver         0 2000     3
    ##    3:     2002 41711    Diesel  90         1   Blue         0 2000     3
    ##    4:     2002 48000    Diesel  90         0  Black         0 2000     3
    ##    5:     2002 38500    Diesel  90         0  Black         0 2000     3
    ##   ---                                                                   
    ## 1432:     1998 20544    Petrol  86         1   Blue         0 1300     3
    ## 1433:     1998 19000    Petrol  86         0   Grey         0 1300     3
    ## 1434:     1998 17016    Petrol  86         0   Blue         0 1300     3
    ## 1435:     1998 16916    Petrol  86         1   Grey         0 1300     3
    ## 1436:     1998     1    Petrol 110         0  Green         0 1600     5
    ##       Cylinders Gears Quarterly_Tax Weight ABS Airbag_1 Airbag_2 Airco
    ##    1:         4     5           210   1165   1        1        1     0
    ##    2:         4     5           210   1165   1        1        1     1
    ##    3:         4     5           210   1165   1        1        1     0
    ##    4:         4     5           210   1165   1        1        1     0
    ##    5:         4     5           210   1170   1        1        1     1
    ##   ---                                                                 
    ## 1432:         4     5            69   1025   1        1        1     1
    ## 1433:         4     5            69   1015   1        1        1     0
    ## 1434:         4     5            69   1015   0        1        1     0
    ## 1435:         4     5            69   1015   0        0        0     0
    ## 1436:         4     5            19   1114   0        1        0     0
    ##       Automatic_airco Boardcomputer CD_Player Central_Lock Powered_Windows
    ##    1:               0             1         0            1               1
    ##    2:               0             1         1            1               0
    ##    3:               0             1         0            0               0
    ##    4:               0             1         0            0               0
    ##    5:               0             1         0            1               1
    ##   ---                                                                     
    ## 1432:               0             0         0            1               1
    ## 1433:               0             0         0            0               0
    ## 1434:               0             0         0            0               0
    ## 1435:               0             0         0            0               0
    ## 1436:               0             0         0            0               0
    ##       Power_Steering Radio Mistlamps Sport_Model Backseat_Divider
    ##    1:              1     0         0           0                1
    ##    2:              1     0         0           0                1
    ##    3:              1     0         0           0                1
    ##    4:              1     0         0           0                1
    ##    5:              1     0         1           0                1
    ##   ---                                                            
    ## 1432:              1     0         1           1                1
    ## 1433:              1     0         0           1                1
    ## 1434:              1     0         0           0                1
    ## 1435:              0     0         0           0                1
    ## 1436:              1     0         0           0                0
    ##       Metallic_Rim Radio_cassette Parking_Assistant Tow_Bar
    ##    1:            0              0                 0       0
    ##    2:            0              0                 0       0
    ##    3:            0              0                 0       0
    ##    4:            0              0                 0       0
    ##    5:            0              0                 0       0
    ##   ---                                                      
    ## 1432:            0              0                 0       0
    ## 1433:            0              0                 0       0
    ## 1434:            0              0                 0       0
    ## 1435:            0              0                 0       0
    ## 1436:            0              0                 0       0
    ##       sq_dev_by_colorl
    ##    1:          7094562
    ##    2:          7178754
    ##    3:          9694267
    ##    4:         15179999
    ##    5:          7269235
    ##   ---                 
    ## 1432:         11131820
    ## 1433:          1171465
    ## 1434:          5458943
    ## 1435:         21877530
    ## 1436:          8329626

``` r
# df[, lapply(.SD, function(x) replace(x, which(x == 0), -1)), .SDcols = 18:35]

for (j in 18:35) {
  set(df,
      i = df[, which(.SD == 0), .SDcols = j],
      j = j,
      value = -1)
}
df
```

    ##                                      Model Price Age_08_04 Mfg_Month
    ##    1:       2.0 D4D HATCHB TERRA 2/3-Doors 13500        23        10
    ##    2:       2.0 D4D HATCHB TERRA 2/3-Doors 13750        23        10
    ##    3:       2.0 D4D HATCHB TERRA 2/3-Doors 13950        24         9
    ##    4:       2.0 D4D HATCHB TERRA 2/3-Doors 14950        26         7
    ##    5:         2.0 D4D HATCHB SOL 2/3-Doors 13750        30         3
    ##   ---                                                               
    ## 1432:          1.3 16V HATCHB G6 2/3-Doors  7500        69        12
    ## 1433: 1.3 16V HATCHB LINEA TERRA 2/3-Doors 10845        72         9
    ## 1434: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  8500        71        10
    ## 1435: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  7250        70        11
    ## 1436:         1.6 LB LINEA TERRA 4/5-Doors  6950        76         5
    ##       Mfg_Year    KM Fuel_Type  HP Met_Color  Color Automatic   CC Doors
    ##    1:     2002 46986    Diesel  90         1   Blue         0 2000     3
    ##    2:     2002 72937    Diesel  90         1 Silver         0 2000     3
    ##    3:     2002 41711    Diesel  90         1   Blue         0 2000     3
    ##    4:     2002 48000    Diesel  90         0  Black         0 2000     3
    ##    5:     2002 38500    Diesel  90         0  Black         0 2000     3
    ##   ---                                                                   
    ## 1432:     1998 20544    Petrol  86         1   Blue         0 1300     3
    ## 1433:     1998 19000    Petrol  86         0   Grey         0 1300     3
    ## 1434:     1998 17016    Petrol  86         0   Blue         0 1300     3
    ## 1435:     1998 16916    Petrol  86         1   Grey         0 1300     3
    ## 1436:     1998     1    Petrol 110         0  Green         0 1600     5
    ##       Cylinders Gears Quarterly_Tax Weight ABS Airbag_1 Airbag_2 Airco
    ##    1:         4     5           210   1165   1        1        1    -1
    ##    2:         4     5           210   1165   1        1        1     1
    ##    3:         4     5           210   1165   1        1        1    -1
    ##    4:         4     5           210   1165   1        1        1    -1
    ##    5:         4     5           210   1170   1        1        1     1
    ##   ---                                                                 
    ## 1432:         4     5            69   1025   1        1        1     1
    ## 1433:         4     5            69   1015   1        1        1    -1
    ## 1434:         4     5            69   1015  -1        1        1    -1
    ## 1435:         4     5            69   1015  -1       -1       -1    -1
    ## 1436:         4     5            19   1114  -1        1       -1    -1
    ##       Automatic_airco Boardcomputer CD_Player Central_Lock Powered_Windows
    ##    1:              -1             1        -1            1               1
    ##    2:              -1             1         1            1              -1
    ##    3:              -1             1        -1           -1              -1
    ##    4:              -1             1        -1           -1              -1
    ##    5:              -1             1        -1            1               1
    ##   ---                                                                     
    ## 1432:              -1            -1        -1            1               1
    ## 1433:              -1            -1        -1           -1              -1
    ## 1434:              -1            -1        -1           -1              -1
    ## 1435:              -1            -1        -1           -1              -1
    ## 1436:              -1            -1        -1           -1              -1
    ##       Power_Steering Radio Mistlamps Sport_Model Backseat_Divider
    ##    1:              1    -1        -1          -1                1
    ##    2:              1    -1        -1          -1                1
    ##    3:              1    -1        -1          -1                1
    ##    4:              1    -1        -1          -1                1
    ##    5:              1    -1         1          -1                1
    ##   ---                                                            
    ## 1432:              1    -1         1           1                1
    ## 1433:              1    -1        -1           1                1
    ## 1434:              1    -1        -1          -1                1
    ## 1435:             -1    -1        -1          -1                1
    ## 1436:              1    -1        -1          -1               -1
    ##       Metallic_Rim Radio_cassette Parking_Assistant Tow_Bar
    ##    1:           -1             -1                -1      -1
    ##    2:           -1             -1                -1      -1
    ##    3:           -1             -1                -1      -1
    ##    4:           -1             -1                -1      -1
    ##    5:           -1             -1                -1      -1
    ##   ---                                                      
    ## 1432:           -1             -1                -1      -1
    ## 1433:           -1             -1                -1      -1
    ## 1434:           -1             -1                -1      -1
    ## 1435:           -1             -1                -1      -1
    ## 1436:           -1             -1                -1      -1
    ##       sq_dev_by_colorl
    ##    1:          7094562
    ##    2:          7178754
    ##    3:          9694267
    ##    4:         15179999
    ##    5:          7269235
    ##   ---                 
    ## 1432:         11131820
    ## 1433:          1171465
    ## 1434:          5458943
    ## 1435:         21877530
    ## 1436:          8329626

## Exercise 9

Use the `set` function to add “yuck\!” to the varible `Fuel_Type` if it
is not `Petrol`. Just
because…

``` r
# df[, lapply(.SD, function(x) replace(x, which(x != "Petrol"), "yuck!")), .SDcol = "Fuel_Type"]

set(df,
    i =  df[, which(Fuel_Type == "Petrol")],
    j = "Fuel_Type",
    value = "Petrol yuck!")
```

## Exercise 10

Using `.SDcols` and one command create two new variables, `log` of
`Weight` and
`Price`.

``` r
df[, (c("logWeight", "logPrice")) := lapply(.SD, log), .SDcols = c("Weight", "Price")]
df
```

    ##                                      Model Price Age_08_04 Mfg_Month
    ##    1:       2.0 D4D HATCHB TERRA 2/3-Doors 13500        23        10
    ##    2:       2.0 D4D HATCHB TERRA 2/3-Doors 13750        23        10
    ##    3:       2.0 D4D HATCHB TERRA 2/3-Doors 13950        24         9
    ##    4:       2.0 D4D HATCHB TERRA 2/3-Doors 14950        26         7
    ##    5:         2.0 D4D HATCHB SOL 2/3-Doors 13750        30         3
    ##   ---                                                               
    ## 1432:          1.3 16V HATCHB G6 2/3-Doors  7500        69        12
    ## 1433: 1.3 16V HATCHB LINEA TERRA 2/3-Doors 10845        72         9
    ## 1434: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  8500        71        10
    ## 1435: 1.3 16V HATCHB LINEA TERRA 2/3-Doors  7250        70        11
    ## 1436:         1.6 LB LINEA TERRA 4/5-Doors  6950        76         5
    ##       Mfg_Year    KM    Fuel_Type  HP Met_Color  Color Automatic   CC
    ##    1:     2002 46986       Diesel  90         1   Blue         0 2000
    ##    2:     2002 72937       Diesel  90         1 Silver         0 2000
    ##    3:     2002 41711       Diesel  90         1   Blue         0 2000
    ##    4:     2002 48000       Diesel  90         0  Black         0 2000
    ##    5:     2002 38500       Diesel  90         0  Black         0 2000
    ##   ---                                                                
    ## 1432:     1998 20544 Petrol yuck!  86         1   Blue         0 1300
    ## 1433:     1998 19000 Petrol yuck!  86         0   Grey         0 1300
    ## 1434:     1998 17016 Petrol yuck!  86         0   Blue         0 1300
    ## 1435:     1998 16916 Petrol yuck!  86         1   Grey         0 1300
    ## 1436:     1998     1 Petrol yuck! 110         0  Green         0 1600
    ##       Doors Cylinders Gears Quarterly_Tax Weight ABS Airbag_1 Airbag_2
    ##    1:     3         4     5           210   1165   1        1        1
    ##    2:     3         4     5           210   1165   1        1        1
    ##    3:     3         4     5           210   1165   1        1        1
    ##    4:     3         4     5           210   1165   1        1        1
    ##    5:     3         4     5           210   1170   1        1        1
    ##   ---                                                                 
    ## 1432:     3         4     5            69   1025   1        1        1
    ## 1433:     3         4     5            69   1015   1        1        1
    ## 1434:     3         4     5            69   1015  -1        1        1
    ## 1435:     3         4     5            69   1015  -1       -1       -1
    ## 1436:     5         4     5            19   1114  -1        1       -1
    ##       Airco Automatic_airco Boardcomputer CD_Player Central_Lock
    ##    1:    -1              -1             1        -1            1
    ##    2:     1              -1             1         1            1
    ##    3:    -1              -1             1        -1           -1
    ##    4:    -1              -1             1        -1           -1
    ##    5:     1              -1             1        -1            1
    ##   ---                                                           
    ## 1432:     1              -1            -1        -1            1
    ## 1433:    -1              -1            -1        -1           -1
    ## 1434:    -1              -1            -1        -1           -1
    ## 1435:    -1              -1            -1        -1           -1
    ## 1436:    -1              -1            -1        -1           -1
    ##       Powered_Windows Power_Steering Radio Mistlamps Sport_Model
    ##    1:               1              1    -1        -1          -1
    ##    2:              -1              1    -1        -1          -1
    ##    3:              -1              1    -1        -1          -1
    ##    4:              -1              1    -1        -1          -1
    ##    5:               1              1    -1         1          -1
    ##   ---                                                           
    ## 1432:               1              1    -1         1           1
    ## 1433:              -1              1    -1        -1           1
    ## 1434:              -1              1    -1        -1          -1
    ## 1435:              -1             -1    -1        -1          -1
    ## 1436:              -1              1    -1        -1          -1
    ##       Backseat_Divider Metallic_Rim Radio_cassette Parking_Assistant
    ##    1:                1           -1             -1                -1
    ##    2:                1           -1             -1                -1
    ##    3:                1           -1             -1                -1
    ##    4:                1           -1             -1                -1
    ##    5:                1           -1             -1                -1
    ##   ---                                                               
    ## 1432:                1           -1             -1                -1
    ## 1433:                1           -1             -1                -1
    ## 1434:                1           -1             -1                -1
    ## 1435:                1           -1             -1                -1
    ## 1436:               -1           -1             -1                -1
    ##       Tow_Bar sq_dev_by_colorl logWeight logPrice
    ##    1:      -1          7094562  7.060476 9.510445
    ##    2:      -1          7178754  7.060476 9.528794
    ##    3:      -1          9694267  7.060476 9.543235
    ##    4:      -1         15179999  7.060476 9.612467
    ##    5:      -1          7269235  7.064759 9.528794
    ##   ---                                            
    ## 1432:      -1         11131820  6.932448 8.922658
    ## 1433:      -1          1171465  6.922644 9.291459
    ## 1434:      -1          5458943  6.922644 9.047821
    ## 1435:      -1         21877530  6.922644 8.888757
    ## 1436:      -1          8329626  7.015712 8.846497
