FROM  mcr.microsoft.com/dotnet/sdk:7.0.407-alpine3.19-amd64 as build
WORKDIR /source
COPY source/* .
RUN dotnet restore
RUN dotnet publish -c release -o /app --self-contained false --no-restore

FROM mcr.microsoft.com/dotnet/runtime:7.0.17-alpine3.19-amd64 as final
RUN adduser --system worker
WORKDIR /app
COPY --from=build /app .
USER worker
ENTRYPOINT ["dotnet", "Worker.dll"]
