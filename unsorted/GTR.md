-------------------------------------------------------
Where we want to get to:

	Beaupre Bus has it's own Identity Server.
	Gateway Trailer has it's own Identity Server.
	Fleet Identity has it's own Identity Server.

	
-------------------------------------------------------
Where we are CURRENTLY ARE: AS DEPLOYED

ids.transportmaintenance.com:
	Referenced by 
		loblaw.transportmaintenance.com
		admin2.gatewaytrailer.net (wash system)
		manage.gatewaytrailer.net (launcher & other apps with reverse dns)

erp.gatewaytrailer.net/ids	
	erp.gatewaytrailer.net/api/sales
	erp.gatewaytrailer.net/sales
	erp.gatewaytrailer.net 
	
* rename admin2.gatewaytrailer.net to wash.gatewaytrailer.net
* api's should only use bearer token
* move warranty from gtr core to gtr erp
* point settings for new warranty to gtr erp
* transfer gatewayusers from ids.transportmaintenance.com to erp.gatewaytrailer.net/ids
* merge gtrgit dev/adminsite to master and DOUBLE CHECK NO MISSING CHANGES ON CURRENT ADMIN SITE
	* gtrgit adminsite needs to be configured to the new erp.gatewaytrailer.net


-------------------------------------------------------
Where we want to get to:

	erp.gatewaytailer.net/api/<application>
	erp.gatewayrailer.net/<application>


-------------------------------------------------------
What we got to clean