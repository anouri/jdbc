# Copyright (C)2015,2016 International Business Machines Corporation and  
# others. All Rights Reserved.    
.PHONY: all clean 

TOOLKIT_NAME=com.ibm.streamsx.jdbc
# Fill in STREAMS_JDBC_TOOLKIT location here.
STREAMS_JDBC_TOOLKIT ?= $(shell  ([ -e "../../$(TOOLKIT_NAME)/toolkit.xml" ] && echo ../../$(TOOLKIT_NAME)) ||\
	         ([ -e "../$(TOOLKIT_NAME)" ] && echo ../$(TOOLKIT_NAME)) ||\
	                 echo $(STREAMS_INSTALL)/toolkits/$(TOOLKIT_NAME))

SPLC_FLAGS ?= -a --data-directory data 
SPLC = $(STREAMS_INSTALL)/bin/sc

SPL_CMD_ARGS ?= -t $(STREAMS_JDBC_TOOLKIT)
SPL_MAIN_COMPOSITE = application::JDBCConnection
#SPL_MAIN_COMPOSITE = com.ibm.streamsx.jdbc.sample.jdbcrun::JDBCRunErrorPort

all: distributed

data: 
	mkdir data

distributed: data
	cd ../com.ibm.streamsx.jdbc; ant
	$(SPLC) $(SPLC_FLAGS) -M $(SPL_MAIN_COMPOSITE) $(SPL_CMD_ARGS)

clean: 
	$(SPLC) $(SPLC_FLAGS) -C -M $(SPL_MAIN_COMPOSITE)
	rm data/*.output
