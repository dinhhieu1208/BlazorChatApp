FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BlazorChatApp/BlazorChatApp.Client/BlazorChatApp.Client.csproj", "BlazorChatApp/BlazorChatApp.Client/"]
RUN dotnet restore "BlazorChatApp/BlazorChatApp.Client/BlazorChatApp.Client.csproj"
COPY . .
WORKDIR "/src/BlazorChatApp/BlazorChatApp.Client"
RUN dotnet build "BlazorChatApp.Client.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "BlazorChatApp.Client.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorChatApp.Client.dll"]
