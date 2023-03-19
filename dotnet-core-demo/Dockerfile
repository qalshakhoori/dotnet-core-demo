#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["dotnet-core-demo/dotnet-core-demo.csproj", "dotnet-core-demo/"]
RUN dotnet restore "dotnet-core-demo/dotnet-core-demo.csproj"
COPY . .
WORKDIR "/src/dotnet-core-demo"
RUN dotnet build "dotnet-core-demo.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "dotnet-core-demo.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dotnet-core-demo.dll"]