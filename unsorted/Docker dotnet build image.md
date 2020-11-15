FROM microsoft/dotnet:2.1.6-sdk-alpine3.7 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

FROM microsoft/dotnet:2.1.6-aspnetcore-runtime-alpine3.7
WORKDIR /app
COPY --from=build-env /app/out .

#ENV ASPNETCORE_URLS=http://*:80
ENTRYPOINT ["dotnet", "Olymel.Profile.Api.dll"]
