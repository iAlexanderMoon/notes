RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs


Dockerfile.dev


Add nodejs to 
```
RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs
```

Self Signed Certificate npm wll still reject it... so we can set an environment variable.

export NODE_EXTRA_CA_CERTS=/usr/local/share/ca-certificates/olymel.sec.crt


Now when you open or rebuild your dev image in the .devcontainer you SHOULD have nodejs installed.





Do we want yarn or npm?



                   (DebugEnsureNodeEnv target) -> 


                     EXEC : error : self signed certificate in certificate chain [/home/moon/working/olywestbs/OBS.Spa/OBS.Spa.csproj]


                     EXEC : error : self signed certificate in certificate chain [/home/moon/working/olywestbs/OBS.Spa/OBS.Spa.csproj]


                     /home/moon/working/olywestbs/OBS.Spa/OBS.Spa.csproj(81,5): error MSB3073: The command "npm install" exited with code 1.
					 
					 
Even though the devcontainer installed it... we need to setup_10

https://stackoverflow.com/questions/57158587/npm-install-error-self-signed-certificate-in-certificate-chain


