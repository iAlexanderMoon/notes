,      

      "Claims": [

        {  "Type": "role", "Value": "cmd" }

      ]
	  
Adding Claims to a Client. Maybe it would be best to give them a role?
"role" "boc_cmd"

Then we can map what a "boc_cmdclient" can do in the application by adding rights to it.


Building command line clients to apis with docker.

Run docker container with dotnet watch to rebuild the clients when something changes.

Use exec into the development container to run the program... we make this simple by adding it to the Makefile

or using our sideload container if it has the environment setup for it!

--add-host on makefile for running the cli of cmd line program this is probably easier.

        --add-host=${HOST_NAME}:${HOST_IP} \ 

You can then dotnet run inside the project folder and pass whatever arguments you want.

http://devopro/boc/api/Series/FXUSDCAD/import?start=2019-09-01&end=2020-01-01
http://devopro/boc/api/Series/FXUSDCAD/import

http://devopro/boc/api/Series/FXUSDCAD/import

http://devopro/api/Series/FXUSDCAD/import