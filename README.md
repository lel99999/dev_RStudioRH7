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

***PQlibVersion Error Fix*** <br/>
```
$sudo ln -s /usr/pgsql-10 /usr/local/postgresql
```

#### For pgsql-10, Error: remotes::install_github("r-dbi/RPostgres") Fix
![https://github.com/lel99999/dev_RStudioRH7/blob/master/RPostgres_ERROR-01.PNG](https://github.com/lel99999/dev_RStudioRH7/blob/master/RPostgres_ERROR-01.PNG) <br/>

```
$export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/pgsql/lib/pkgconfig
$export PATH=$PATH:/usr/pgsql/lib/pkgconfig
sudo R -e 'remotes::install_github("r-dbi/RPostgres")' --configure-vars='INCLUDE_DIR=/usr/pgsql/bin:/usr/pgsql-10/include'
```

#### R Connection String using PostgreSQL Driver
```
>con <- dbConnect(RPostgres::Postgres(),dbname = 'testdb', host = 'testhost.example.com', port = '5432', user = '<username>', password = '*****')
>test_q <- dbSendQuery(con,'SELECT * from public.test;')
>dbFetch(test_q)

```

#### R Connection String using ODBC DSN
```
>con1 <- DBI::dbConnect(odbc::odbc(),dsn='<dsn_name>',uid='<username>',pwd='*****')
>test_q1 <- dbSendQuery(con1,'SELECT * from public.test;')
>dbFetch(test_q1)
```
#### Error: could not find function "dbConnect"
```
>library(DBI)
```
#### isql Error
```
$isql -v <dsn_name>
[08001][unixODBC]could not connect to server: No such file or directory
	Is the server running locally and accepting
	connections on Unix domain socket "/var/run/postgresql/.s.PGSQL.5432"?
```
***isql Fix*** <br/>
```
$sudo yum install libiodbc
```
#### RStudio Error for hadabend session error
Fix is to mv  ~/.rstudio to ~/.rstudio_bkp <br/>
