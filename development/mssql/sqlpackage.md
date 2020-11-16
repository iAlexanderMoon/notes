sqlpackage.exe 
	/a:Import 
	/tsn:tcp:<ServerName>.database.windows.net,1433 
	/tdn:<TargetDatabaseName> 
	/tu:sa 
	/tp:Olymel20192019 
	/sf:<Path to bacpac file> 
	
sqlpackage.exe 
	/a:Export
	/scs:"Data Source=sqlserver;Initial Catalog=procurement;Integrated Security=False;User ID=sa;Password=Olymel20192019"
	/tf:"database_backup.bacpac"
	
	   "IPAddress": "172.17.0.2",

	   
run-export-procurement:
        docker run --name sqlpackage --rm \
        -v /home/ian:/home/ian guestline/sqlpackage "/a:Export" \
        /scs:"Data Source=${SQLSERVER};Initial Catalog=procurement;Integrated Security=False;User ID=${USERID};Password=${PASSWORD}" \
        /tf:/home/ian/procurement_${EXPORT}.bacpac
