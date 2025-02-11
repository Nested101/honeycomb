FROM mcr.microsoft.com/dotnet/sdk:6.0.100-rc.2 AS build
WORKDIR /source

# copy csproj and restore as distinct layers
COPY *.sln .
COPY .editorconfig .
COPY Directory.Build.props .
COPY global.json .
COPY nuget.config .
COPY src/HoneyComb.Shared/*.csproj ./src/HoneyComb.Shared/
COPY src/HoneyComb.API/*.csproj ./src/HoneyComb.API/
COPY src/HoneyComb.Server/*.csproj ./src/HoneyComb.Server/
RUN dotnet restore

# copy everything else and build app
COPY . .
WORKDIR /source/src/HoneyComb.API
RUN dotnet publish -c release -o /app --no-restore

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:6.0.0-rc.2
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["dotnet", "HoneyComb.API.dll"]
