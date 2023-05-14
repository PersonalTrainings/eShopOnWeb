#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/nightly/sdk:8.0-preview AS build
ARG TARGETARCH
WORKDIR /app
COPY . .
#COPY ["src/PublicApi/PublicApi.csproj", "./PublicApi/"]
#RUN dotnet restore "./PublicApi/PublicApi.csproj"
#COPY . .
WORKDIR "/app/src/PublicApi"
RUN dotnet restore -a $TARGETARCH

RUN dotnet build "./PublicApi.csproj" -c Release -a $TARGETARCH -o /app/build

FROM build AS publish
RUN dotnet publish "./PublicApi.csproj" -c Release -a $TARGETARCH -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PublicApi.dll"]