#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["blazor-wasm-app/Server/blazor-wasm-app.Server.csproj", "blazor-wasm-app/Server/"]
COPY ["blazor-wasm-app/Client/blazor-wasm-app.Client.csproj", "blazor-wasm-app/Client/"]
COPY ["blazor-wasm-app/Shared/blazor-wasm-app.Shared.csproj", "blazor-wasm-app/Shared/"]
RUN dotnet restore "blazor-wasm-app/Server/blazor-wasm-app.Server.csproj"
COPY . .
WORKDIR "/src/blazor-wasm-app/Server"
RUN dotnet build "blazor-wasm-app.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "blazor-wasm-app.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "blazor-wasm-app.Server.dll"]
