# dev_RStudioRH7
RStudio Development/Deployment Workspace, Deployment, and Notes for RedHat 7.x
- RStudio Desktop -> v1.4.1106
- RStudio Server -> v.1.4.1106
- RStudio Server Configuration

#### RStudio Products
- Shiny Server -> v1.5.16.958
- R Packages
- shinyapps.io <br/>

#### Open Source

#### Professional
- RStudio Server Pro
- RStudio Connect
- RStudio Package Manager

#### Launch RStudio
If launch error like the following occurs: <br/>
![https://github.com/lel99999/dev_RStudioRH7/blob/master/rstudio_error-01b.png](https://github.com/lel99999/dev_RStudioRH7/blob/master/rstudio_error-01b.png) <br/>

Launch RStudio with the following: <br/>

```
$ssh -Y <host> QMLSCENE_DEVICE=software rstudio
```

#### Updated for tidycensus
- install/scl enable devtools-8
- configure/make/make install gdal223
- copy related libraries to /lib64
- Install sf package
- Install tidycensus package

```
$sudo cp /usr/local/lib/libkml* /lib64/
$sudo cp /usr/local/lib/liburiparser* /lib64/
$sudo cp /usr/local/lib/libminizip* /lib64/

$sudo R -e 'install.packages("sf",repo="https://CRAN.R-project.org")'
$sudo R -e 'install.packages("tidycensus",repo="https://CRAN.R-project.org")'
```

#### Install RPostgreSQL from CRAN and RPostgres from Gibhub
```
>install.packages('RPostgreSQL')
```

***Install from CRAN*** <br/>
```
>install.packages('devtools')
>install.packages('remotes')

>remotes::install_github('r-dbi/RPostgres")


```
