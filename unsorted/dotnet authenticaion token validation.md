Specifying the default scheme results in the HttpContext.User property being set to that identity. If that behavior isn't desired, disable it by invoking the parameterless form of AddAuthentication.

https://docs.microsoft.com/en-us/aspnet/core/security/authorization/limitingidentitybyscheme?view=aspnetcore-3.1

MAKE SURE YOU SPECIFY THE DEFAULT SCHEME!


            services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
				
                .AddJwtBearer(options => {
                    options.Audience = audience;
                    options.Authority = authority;
                    options.RequireHttpsMetadata = requireHttpsMetadata;
                });

				
				    options.TokenValidationParameters = new TokenValidationParameters
    {
        // Clock skew compensates for server time drift.
        // We recommend 5 minutes or less:
        ClockSkew = TimeSpan.FromMinutes(5),
        // Specify the key used to sign the token:
        IssuerSigningKey = signingKey,
        RequireSignedTokens = true,
        // Ensure the token hasn't expired:
        RequireExpirationTime = true,
        ValidateLifetime = true,
        // Ensure the token audience matches our audience value (default true):
        ValidateAudience = true,
        ValidAudience = "api://default",
        // Ensure the token was issued by a trusted authorization server (default true):
        ValidateIssuer = true,
        ValidIssuer = "https://{yourOktaDomain}/oauth2/default"
    };