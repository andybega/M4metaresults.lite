
<!-- README.md is generated from README.Rmd. Please edit that file -->
M4metaresults.lite
==================

M4metaresults.lite is a lightweight version of [m4metaresults](https://github.com/pmontman/M4metaresults) that only includes the trained model file neccessary to use the [M4metalearning](https://github.com/robjhyndman/M4metalearning) model. It does not use lazy loading in order to speed up package installation and fix an install error in docker. As a result it requires explicitly loading the data with `data("model_M4")`, unlike the original package.

Example, taken from the [M4meta reproduction script](https://github.com/robjhyndman/M4metalearning/blob/master/docs/M4_reprod.md):

``` r
library("M4metalearning")
library("M4metaresults.lite")
data("model_M4")

set.seed(10-06-2018)
truex = (rnorm(60)) + seq(60)/10

#we subtract the last 10 observations to use it as 'true future' values
#and keep the rest as the input series in our method
h = 10
x <- head(truex, -h)
x <- ts(x, frequency = 1)

#forecasting with our method using our pretrained model in one line of code
#just the input series and the desired forecasting horizon
forec_result <- forecast_meta_M4(model_M4, x, h=h)
#> Loading required package: tsfeatures
forec_result
#> $mean
#>  [1] 5.257107 5.322491 5.386980 5.452210 5.517476 5.582802 5.648146
#>  [8] 5.713506 5.778880 5.844265
#> 
#> $upper
#>  [1]  8.145680  8.636207  8.935882  9.510839  9.664657 10.685227  9.787108
#>  [8]  9.895157 10.042725 10.221341
#> 
#> $lower
#>  [1] 2.3685340 2.0087750 1.8380786 1.3935814 1.3702946 0.4803768 1.5091841
#>  [8] 1.5318561 1.5150337 1.4671892
```

Install
-------

From github:

``` r
library("remotes")
remotes::install_github("andybega/M4metaresults.lite@v0.1.0")
```

Note on data
------------

Since the data file is 800MB, it is not possible to include it on GitHub except under the release tarball.

By default R's `save()` uses compression. The model file is 800MB in memory, and similar on disk if compression is used. However, this results in atrocious load times. Saving with `save(..., compress = FALSE)` increases the file size on disk to 1.6GB, but the load time decreases from 7s to 2s.

I removed lazy load from the DESCRIPTION file, this skips the respective install portion and make install much faster. The package will in any case only be loaded when needed.
