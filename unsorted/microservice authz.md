endpointfactoy.service.ts  

getLoginEndpoint

should these be hardcoded?: clientid, grant_type, scopes ... 

   const params = new HttpParams()
            .append('username', userName)
            .append('password', password)
            .append('client_id', 'obs_spa')
            .append('grant_type', 'password')
            .append('scope', 'profile email roles');
			
--------------------------------------------------------------------------------
Custom Identity Server Endpoint for impersonation... a good or a bad idea?
			
getLoginAsEndpoint	
Custom Endpoint added to identity server: /impersonation/post


--------------------------------------------------------------------------------
Access Token is assumed to be carrying the following properties for the client to use... 
but that's not really the way that it's supposed to work is it

Why would you put so much in the AccessToken. Anyway let's try and provide fullname and name


      decodedAccessToken.sub,
      decodedAccessToken.name,
      decodedAccessToken.fullname,
      decodedAccessToken.email,
      decodedAccessToken.jobtitle,
      decodedAccessToken.phone_number,
	  
user.model.ts


------------------------------------------------------------------------------
After getting a token it requests the /obsapi/api/account/userinfo

Returns the uid and list of rights that is used by the spa to figure out what the user is authorized to do for the gui.

Claims are loaded for the user through a Claims Transformer which maps roles from the access token to Rights.
Rights are statically mapped from RolesToRights.

------------------------------------------------------------------------------

How do you manage who can confirm a load for a producer?

------------------------------------------------------------------------------







	