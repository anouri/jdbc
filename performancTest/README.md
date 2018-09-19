# SPL application to perform *Performance Test* using JDBC toolkit 

This document describes a step by step instruction to create and configure a SPL application to perform *Performance Test* using JDBC toolkit.

The SPL application is created in mixed mode (SPLM) and uses the streams JDBC toolkit to connect to a database.

It reads the configuration file *config.cfg* and crates a SPL file depending to the configurtion parameter *parallelLoaders* 

It creates several connections to a DB2 database via DB2 JDBC driver and insert parallel data into several tables in database.


## 1 - Download the DB2 jdbc driver (db2jcc4.jar) from:

http://www-01.ibm.com/support/docview.wss?uid=swg21363866

## 2 - Create a database and a test table in your DB2 database 
login as db2inst1 on your DB2 server

 create a database for example TESTDB2

 create a test table and insert some data into table.
 


    db2 create database TESTDB2
    DB20000I  The CREATE DATABASE command completed successfully.

    db2 connect to TESTDB2 user db2inst1 using db2passwd
    Database Connection Information
    Database server        = DB2/LINUXX8664 11.1.1
    SQL authorization ID   = DB2INST1
    Local database alias   = TESTDB2

    db2 "create table test (name varchar(30), id int)";
    DB20000I  The SQL command completed successfully.
    
    db2 "insert into test values ('jim', 1)";
    DB20000I  The SQL command completed successfully.
    db2 "insert into test values ('kati', 2)";
    DB20000I  The SQL command completed successfully.
     
    db2 "select * from  test";
    NAME                 ID         
    -------------------- -----------
    jim                            1
    kati                           2
    2 record(s) selected.



## 3 - configuration
Change the database credentials in config.cfg file with your database credentials

jdbcUrl=jdbc:db2://<your db2 database host name>:50000/TESTDB2
jdbcUser=<your database user name>
jdbcPassword=<your database password>

This configuration file shows the configuration for a DB2 database.

It is also possible to configure the SPL application with other databases.


## 4 - Make the SPL application

  check the Makefile
  and run make

    #####################################################################
    # Copyright (C)2014, 2017 International Business Machines Corporation and
    # others. All Rights Reserved.
    #####################################################################
    
    .PHONY: all clean
    
    SPLC_FLAGS = -t $(STREAMS_INSTALL)/toolkits/com.ibm.streamsx.jdbc  --data-directory data
    #SPLC_FLAGS = -t ../streamsx.jdbc/com.ibm.streamsx.jdbc  --data-directory data
    
    SPLC = $(STREAMS_INSTALL)/bin/sc
    
    SPL_CMD_ARGS ?=
    SPL_COMP1NAME=JDBCPerform 
    SPL_MAIN_COMPOSITE1 = application::$(SPL_COMP1NAME)
    BUILD_OUTPUT_DIR = output
    
    all: data clean
    	$(SPLC) $(SPLC_FLAGS) -M  $(SPL_MAIN_COMPOSITE1) --output-dir ./$(BUILD_OUTPUT_DIR)  $(SPL_CMD_ARGS)
    
    data:
    	mkdir data
    clean:
    	$(SPLC) $(SPLC_FLAGS) -C -M $(SPL_MAIN_COMPOSITE1) --output-dir output
    	-rm -rf toolkit.xml
    	-rm -rf data/output.dat
    

     

## 5 - Run the SPL application 
Check if the Streams domain and streams instance are already running healthy

Start the application with 

     $> streamtool submitjob output/application.JDBCPerform.sab

## 6 - check teh test result 
On databse server     

     $> db2 connect to TESTDB2 user db2inst1 using db2passwd
     $> db2 "SELECT count(*) FROM JDBCRUN_PERSON2"

On Streams server:

     $ streamtool lspes
     $ top








