#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# FROM mcr.microsoft.com/azure-functions/dotnet:4 AS base
#WORKDIR /home/site/wwwroot
#EXPOSE 80

#FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
#WORKDIR /src
#COPY ["HelloWorld.csproj", "."]
#RUN dotnet restore "./HelloWorld.csproj"
#RUN dotnet restore "HelloWorld.csproj"
#COPY . .
#WORKDIR "/src/."
#RUN dotnet build "HelloWorld.csproj" -c Release -o /app/build

#FROM build AS publish
#RUN dotnet publish "HelloWorld.csproj" -c Release -o /app/publish /p:UseAppHost=false

#FROM base AS final
#WORKDIR /home/site/wwwroot
#COPY --from=publish /app/publish .
#ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
#    AzureFunctionsJobHost__Logging__Console__IsEnabled=true

FROM registry.access.redhat.com/ubi8/dotnet-60:6.0-25 as builder
WORKDIR /opt/app-root/src
COPY --chown=1001 . .
RUN dotnet publish -c Release src/HelloWorld/HelloWorld.sln


FROM registry.access.redhat.com/ubi8/dotnet-60:6.0-25
EXPOSE 80
ENV ASPNETCORE_URLS=http://*:80
COPY --from=builder /opt/app-root/src/bin /opt/app-root/src/bin
WORKDIR /opt/app-root/src/bin/Release/net6.0/publish
CMD ["dotnet", "app.dll"]
