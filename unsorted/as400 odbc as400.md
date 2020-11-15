docker run -it microsoft/dotnet:2.1-sdk-alpine /bin/bash


	docker exec -it microsoft/dotnet:2.1-sdk-alpine /bin/bash
 
 
[04:30:06 ERR] An unhandled exception has occurred while executing the request.


System.DllNotFoundException: Dependency unixODBC with minimum version 2.3.1 is required.


Unable to load shared library 'libodbc.so.2' or one of its dependencies. In order to help diagnose loading problems, consider setting the LD_DEBUG environment variable: Error loading shared library liblibodbc.so.2.so: No such file or directory


[05:01:00 ERR] An unhandled exception has occurred while executing the request.


System.Data.Odbc.OdbcException (0x80131937): ERROR [01000] [unixODBC][Driver Manager]Can't open lib 'Client Access ODBC Driver' : file not found