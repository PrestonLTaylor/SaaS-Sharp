FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Restore 
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/Domain/Domain.csproj", "src/Domain/"]
COPY ["src/Application/Application.csproj", "src/Application/"]
COPY ["src/Infrastructure/Infrastructure.csproj", "src/"]
COPY ["src/Presentation.Client/Presentation.Client.csproj", "src/Presentation.Client/"]
COPY ["src/Presentation.Server/Presentation.Server.csproj", "src/Presentation.Server/"]
RUN dotnet restore "./src/Presentation.Server/Presentation.Server.csproj"
COPY . .

# Build
WORKDIR "/src/src/Presentation.Server"
RUN dotnet build "./Presentation.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Presentation.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Execute
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Presentation.Server.dll"]