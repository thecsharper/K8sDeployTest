#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0-preview AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0-preview AS build
WORKDIR /src
COPY ["K8sDeployTest/K8sDeployTest.csproj", "K8sDeployTest/"]
RUN dotnet restore "K8sDeployTest/K8sDeployTest.csproj"
COPY . .
WORKDIR "/src/K8sDeployTest"
RUN dotnet build "K8sDeployTest.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "K8sDeployTest.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "K8sDeployTest.dll"]