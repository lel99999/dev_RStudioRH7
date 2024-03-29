#### dev_RStudioRH7
RStudio Development/Deployment Workspace, Deployment, and Notes for RedHat 7.x
Latest:
- RStudio Desktop -> https://s3.amazonaws.com/rstudio-ide-build/electron/centos7/x86_64/rstudio-2023.06.0-421-x86_64.rpm
- RStudio Server -> https://download2.rstudio.org/server/centos7/x86_64/rstudio-server-rhel-2023.06.0-421-x86_64.rpm
  
Previous:
- RStudio Desktop -> v1.4.1106 -> v2022.07.1-554 
- RStudio Server -> v.1.4.1106 -> v2022.07.1-554
- RStudio Server Configuration -> v2022.07.1-554

##### TODO
- Review Running RStudio with different versions of R v3.6, v4.x and capability to switch
- Investigate Renv & renv for multiple runtime shims

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

#### RStudio XWindows Error
- review /etc/ssh/sshd_config <br/>
- X11Forwarding on Server and Client

#### Suppress RStudio QT Error 
- Set env QT_LOGGING_RULES
  `$ export QT_LOGGING_RULES='*.debug=false;qt.qpa.*=false'` <br/>

#### RStudio libGL Error:
- ![RStudio libGL Error](https://github.com/lel99999/dev_RStudioRH7/blob/master/rstudio-libGL-errors-01.PNG) <br/>

- Debug: $export LIBGL_DEBUG=verbose
  ![RStudio libGL Debug](https://github.com/lel99999/dev_RStudioRH7/blob/master/rstudio-libGL-debug-01a.png) <br/> 

#### Scripting and Command-line frontend for R (littler)
- [http://dirk.eddelbuettel.com/code/littler.html](http://dirk.eddelbuettel.com/code/littler.html) <br/>

#### Install lwgeom
- [https://cran.r-project.org/web/packages/lwgeom/lwgeom.pdf](https://cran.r-project.org/web/packages/lwgeom/lwgeom.pdf) <br/>
  ```
  $sudo R -e 'install.packages("lwgeom",repos="https://cran.r-project.org")'
  ```
#### Build geos geos-devel from source
- [https://cmake.org/download/](https://cmake.org/download/) <br/>
- [https://libgeos.org/usage/download/](https://libgeos.org/usage/download/) <br/>

#### Use Doxygen for API Documentation
- [https://www.doxygen.nl/](https://www.doxygen.nl/) <br/>

#### Updating PROJ -> 6.2.x and SQLite -> 3.11.x for RHEL7
- [http://sqlite.org/2016/sqlite-autoconf-3110000.tar.gz](http://sqlite.org/2016/sqlite-autoconf-3110000.tar.gz) <br/>
  ```
  $./configure --prefix=/usr --disable-static        \
            CFLAGS="-g -O2 -DSQLITE_ENABLE_FTS3=1 \
            -DSQLITE_ENABLE_COLUMN_METADATA=1     \
            -DSQLITE_ENABLE_UNLOCK_NOTIFY=1       \
            -DSQLITE_SECURE_DELETE=1              \
            -DSQLITE_ENABLE_DBSTAT_VTAB=1" && make -j1


  $export SQLITE3_LIBS="-L/usr/lib -lsqlite3"
  ```
#### Update Rcpp
```
$sudo R -e 'install.packages("Rcpp", repos="https://RcppCore.github.io/drat")'
```
#### Install rstan on RHEL 7.9
- [https://mc-stan.org/](https://mc-stan.org/) <br/>
- [https://github.com/stan-dev/rstan](https://github.com/stan-dev/rstan) <br/>
- Using >= devtoolset-7 <br/>
  ```
  $scl enable devtoolset-7 bash
  ```
- ERROR: C++14 standard requested but CXX14 is not defined <br/>
  - FIX:  Update R_Makevars add below
  ```
  ## C++ flags
  CXX=g++
  CXX11=g++
  CXX14=g++
  CXX17=g++
  ```
