# checkConnection

The **JDBCRun** operator has been improved in version **1.4.0** with a new parameter checkConnection.

https://github.com/IBMStreams/streamsx.jdbc/releases/tag/v1.4.0

This optional parameter specifies whether a checkConnection thread should be start.
It checks periodically the status of JDBC connection.
The JDBCRun sends in case of any failure a SqlCode and a message to SPL application.

The sample JDBCConnection demonstrates a JDBCRun opeartor with **checkConnection** parameter
