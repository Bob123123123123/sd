# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /source
# copy csproj and restore as distinct layers
COPY *.sln .
COPY server/*.csproj ./server/
RUN dotnet restore -r linux-x64

# copy everything else and build app
COPY server/. ./server/
WORKDIR /source/server
RUN dotnet publish -c release -o /app -r linux-x64 --self-contained false
# dotnet dev-certs https --trust

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:5.0-focal-amd64
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["./server"] 