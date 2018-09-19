.PHONY: all distributed clean 

JDBC_TOOLKIT_INSTALL = $(STREAMS_INSTALL)/toolkits/com.ibm.streamsx.jdbc
#JDBC_TOOLKIT_INSTALL = ../com.ibm.streamsx.jdbc
SPLC_FLAGS ?= -a
SPLC = $(STREAMS_INSTALL)/bin/sc

SPL_CMD_ARGS ?= -t $(JDBC_TOOLKIT_INSTALL)
SPL_MAIN_COMPOSITE = application::JDBCHbase

all: distributed

distributed:
#	cd ../com.ibm.streamsx.jdbc; ant; 
	JAVA_HOME=$(STREAMS_INSTALL)/java $(SPLC) $(SPLC_FLAGS) -M $(SPL_MAIN_COMPOSITE) $(SPL_CMD_ARGS) --data-directory data

clean: 
	$(SPLC) $(SPLC_FLAGS) -C -M $(SPL_MAIN_COMPOSITE)
	rm -rf output
	rm -rf bin
