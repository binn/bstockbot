#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:7.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["BStockBot.Worker/BStockBot.Worker.csproj", "BStockBot.Worker/"]
RUN dotnet restore "BStockBot.Worker/BStockBot.Worker.csproj"
COPY . .
WORKDIR "/src/BStockBot.Worker"
RUN dotnet build "BStockBot.Worker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BStockBot.Worker.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BStockBot.Worker.dll"]