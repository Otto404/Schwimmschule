# Unter https://aka.ms/customizecontainer erfahren Sie, wie Sie Ihren Debugcontainer anpassen und wie Visual Studio dieses Dockerfile verwendet, um Ihre Images für ein schnelleres Debuggen zu erstellen.

# Diese ARGs ermöglichen den Austausch der Basis, die zum Erstellen des endgültigen Images beim Debuggen von VS verwendet wird.
ARG LAUNCHING_FROM_VS
# Hiermit wird das Basisimage für "final" festgelegt, jedoch nur, wenn LAUNCHING_FROM_VS definiert wurde.
ARG FINAL_BASE_IMAGE=${LAUNCHING_FROM_VS:+aotdebug}

# Diese Stufe wird verwendet, wenn sie von VS im Schnellmodus ausgeführt wird (Standardeinstellung für Debugkonfiguration).
FROM mcr.microsoft.com/dotnet/aspnet:9.0-preview AS base
USER app
WORKDIR /app
EXPOSE 8080


# Diese Stufe wird zum Erstellen des Dienstprojekts verwendet.
FROM mcr.microsoft.com/dotnet/sdk:9.0-preview AS build

WORKDIR /src
COPY Schwimmschule.csproj .
RUN dotnet restore Schwimmschule.csproj
COPY . .
RUN dotnet build Schwimmschule.csproj -c Release -o /app/build

FROM build AS publish
RUN dotnet publish Schwimmschule.csproj -c Release -o /app/publish

FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY nginx.conf /etc/nginx/nginx.conf