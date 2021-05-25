#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/WorkerService/LetsDoIt.WTE.WS.Worker/LetsDoIt.WTE.WS.Worker.csproj", "src/WorkerService/LetsDoIt.WTE.WS.Worker/"]
RUN dotnet restore "src/WorkerService/LetsDoIt.WTE.WS.Worker/LetsDoIt.WTE.WS.Worker.csproj"
COPY . .
WORKDIR "/src/src/WorkerService/LetsDoIt.WTE.WS.Worker"
RUN dotnet build "LetsDoIt.WTE.WS.Worker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "LetsDoIt.WTE.WS.Worker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "LetsDoIt.WTE.WS.Worker.dll"]