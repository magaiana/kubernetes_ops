#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["kubernetes_ops.service/kubernetes_ops.service.csproj", "kubernetes_ops.service/"]
RUN dotnet restore "kubernetes_ops.service/kubernetes_ops.service.csproj"
COPY . .
WORKDIR "/src/kubernetes_ops.service"
RUN dotnet build "kubernetes_ops.service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "kubernetes_ops.service.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "kubernetes_ops.service.dll"]