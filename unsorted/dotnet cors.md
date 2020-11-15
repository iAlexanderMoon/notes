Cross Origin Requests: CORS:

TLDR;

Browser security prevents a web page from making requests to a different domain than the one that served the web page.

	When you read that you think the browser DOESN'T make the request... but that's not what actually happens... the browser makes the API 

CORS gives you the power to allow cross-domain calls to the api from the specified domains... but that HARDLY SEEMS LIKE THE BROWSER REJECTING IT?
WHY DO ALL ARTICLES REFER TO THE BROWSER AS ABOVE?

API:

* services must be registered
```
services.AddCors(options =>
        {
            options.AddPolicy(MyAllowSpecificOrigins,
            builder =>
            {
                builder.WithOrigins("http://example.com",
                                    "http://www.contoso.com");
            });
        });
```
* middleware applies cors to ALL endpoints in the api
* position is important
```
	app.UseRouting();
	app.UseCors()
	app.UseEndpoints(

	Web Page:
```


References:

https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-3.1