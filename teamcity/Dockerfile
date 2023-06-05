#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["teamcity/teamcity.csproj", "teamcity/"]
RUN dotnet restore "teamcity/teamcity.csproj"
COPY . .
WORKDIR "/src/teamcity"
RUN dotnet build "teamcity.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "teamcity.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "teamcity.dll"]