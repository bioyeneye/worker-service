#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/runtime:3.1-buster-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["WorkerServiceSample/WorkerServiceSample.csproj", "WorkerServiceSample/"]
RUN dotnet restore "WorkerServiceSample/WorkerServiceSample.csproj"
COPY . .
WORKDIR "/src/WorkerServiceSample"
RUN dotnet build "WorkerServiceSample.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WorkerServiceSample.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WorkerServiceSample.dll"]