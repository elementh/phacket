FROM mcr.microsoft.com/dotnet/sdk:7.0-alpine AS build
WORKDIR /src

COPY *.sln ./

COPY Phacket.Api/*.csproj Phacket.Api/

RUN dotnet restore -r linux-musl-x64

COPY . .
WORKDIR /src/Phacket
RUN dotnet publish -c Release -o /app -r linux-musl-x64 --self-contained false --no-restore ../Phacket.sln 

FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine
WORKDIR /app
COPY --from=build /app ./

ENV ASPNETCORE_ENVIRONMENT=Production
ENV ASPNETCORE_URLS http://*:5100
EXPOSE 5100

ENTRYPOINT ["dotnet", "Phacket.Api.dll"]
