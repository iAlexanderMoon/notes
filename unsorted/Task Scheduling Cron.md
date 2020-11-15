Crontab to run jobs on a schedule in a production like environment.


execute a cronjob in demand. 
It doesn't look like you can really.

It's probably better to maintain a folder of scripts (again mounted into the container or copied in for production so they aren't editable in production) that way the scripts can be executed manually.

Cron Scripts Logs Last Runs etc.

Cron doesn't record success failure except to log to syslog.
We could write logs runs and results etc to files for each run:

Mount /data/cron folder

&& date >> /cron/<jobname>.log 


Requests to API Endpoints

Can curl be used to authenticate to identity server to get an access token or a refresh token?
https://github.com/IdentityServer/IdentityServer4/issues/2633

Serilog or other consolidated logging?
Correlation Id?

curl -X POST "http://devopro/boc/api/Series/FXUSDCAD/import?start=2019-01-01" -H  "accept: application/json"
