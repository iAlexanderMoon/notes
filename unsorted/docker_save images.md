docker save <image> | bzip2 | pv | ssh ian@192.168.63.4 'bunzip2 | docker load'

olymel/vuenpm                    0.1                                  2b423551c4c4        7 hours ago         158MB
olymel/vueyarn                   0.1                                  c91b0d2c0e44        8 hours ago         243MB
olymel/vueyarn                   latest                               c91b0d2c0e44        8 hours ago         243MB
microsoft/dotnet                 2.1-sdk-alpine                       85a637452a6a        13 days ago         1.47GB
microsoft/dotnet                 2.1-sdk-alpine3.7                    85a637452a6a        13 days ago         1.47GB
microsoft/dotnet                 2.1.6-aspnetcore-runtime-alpine3.7   3851ccf55966        13 days ago         161MB
traefik                          v1.7.6-alpine                        9e8d573a4f30        2 weeks ago         74MB
mcr.microsoft.com/mssql/server   2017-CU12-ubuntu                     4095d6d460cd        2 months ago        1.32GB
node                             10.11.0-alpine                       5206c0dd451a        3 months ago        69.9MB
alpine/socat                     latest       