#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["BlazorPortfolioBlog/Server/BlazorPortfolioBlog.Server.csproj", "BlazorPortfolioBlog/Server/"]
COPY ["BlazorPortfolioBlog/Client/BlazorPortfolioBlog.Client.csproj", "BlazorPortfolioBlog/Client/"]
COPY ["BlazorPortfolioBlog/Shared/BlazorPortfolioBlog.Shared.csproj", "BlazorPortfolioBlog/Shared/"]
RUN dotnet restore "BlazorPortfolioBlog/Server/BlazorPortfolioBlog.Server.csproj"
COPY . .
WORKDIR "/src/BlazorPortfolioBlog/Server"
RUN dotnet build "BlazorPortfolioBlog.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorPortfolioBlog.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorPortfolioBlog.Server.dll"]