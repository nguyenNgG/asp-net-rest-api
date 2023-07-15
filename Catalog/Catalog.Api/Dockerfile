# first stage: base
# get the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
# overwrite port to be 80
EXPOSE 80

ENV ASPNETCORE_URLS=http://+:80

# second stage: build
# get the sdk image
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
# copy file to current directory
COPY ["Catalog.Api.csproj", "./"]
# get dependencies
RUN dotnet restore "Catalog.Api.csproj"
# copy all files
COPY . .
# build and publish with configuration for release into folder 
RUN dotnet publish "Catalog.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

# third stage: final
FROM base AS final
WORKDIR /app
# wait for build stage to finish
# copy files from build stage into the current directory
COPY --from=build /app/publish .
# command to start the REST API
ENTRYPOINT ["dotnet", "Catalog.Api.dll"]
