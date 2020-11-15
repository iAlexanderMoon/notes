Identity Server 4

The AddInMemoryClients extensions method also supports adding clients from the ASP.NET Core configuration file. 
This is the way to do it for clients. A restart would be necessary but how often would these be changing.... not often.

Clients: We need an excel client and a web client (mvc for now) no spa.
We don't need to sign in a user because we know which user of the computer is being used anyway because it's only 1 connection to prophetx.
No specific user. But the client is specific.



public class Clients
{
    public static IEnumerable<Client> Get()
    {
        return new List<Client>
        {
            new Client
            {
                ClientId = "service.client",
                ClientSecrets = { new Secret("secret".Sha256()) },

                AllowedGrantTypes = GrantTypes.ClientCredentials,
                AllowedScopes = { "api1", "api2.read_only" }
            }
        };
    }
}

Defining clients in appsettings.json


Excel:

https://github.com/VBA-tools/VBA-Web/wiki/Authentication
OAuth 2.0 - Client-credentials flow

So 1) Stand up a local test api with identity server 4
Connect excel to it.

Build it for dockerhub
publish to digital ocean