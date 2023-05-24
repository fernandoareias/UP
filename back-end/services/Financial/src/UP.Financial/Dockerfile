#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0-preview AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0-preview AS build
WORKDIR /src
COPY ["services/Financial/src/UP.Financial/UP.Financial.csproj", "services/Financial/src/UP.Financial/"]
RUN dotnet restore "services/Financial/src/UP.Financial/UP.Financial.csproj"
COPY . .
WORKDIR "/src/services/Financial/src/UP.Financial"
RUN dotnet build "UP.Financial.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "UP.Financial.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "UP.Financial.dll"]