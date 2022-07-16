#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

USER root
RUN curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-17.04.0-ce.tgz \
  && tar xzvf docker-17.04.0-ce.tgz \
  && mv docker/docker /usr/local/bin \
  && rm -r docker docker-17.04.0-ce.tgz

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/PedeLogo.Catalogo.Api/PedeLogo.Catalogo.Api.csproj", "src/PedeLogo.Catalogo.Api/"]
RUN dotnet restore "src/PedeLogo.Catalogo.Api/PedeLogo.Catalogo.Api.csproj"
COPY . .
WORKDIR "/src/src/PedeLogo.Catalogo.Api"
RUN dotnet build "PedeLogo.Catalogo.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PedeLogo.Catalogo.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PedeLogo.Catalogo.Api.dll"]