FROM mcr.microsoft.com/dotnet/sdk:9.0.202 AS sdk
WORKDIR /src_playwright
RUN dotnet new console --no-restore 
RUN dotnet add package Microsoft.Playwright
RUN dotnet build --property:PlaywrightPlatform=all -c Release

FROM registry.hub.docker.com/mu88/devcontainers-dotnet:latest AS playwright-install
COPY --from=sdk /src_playwright/bin/Release/net*/ /src/
ENV PLAYWRIGHT_BROWSERS_PATH=/ms-playwright
RUN mkdir /ms-playwright
RUN ["pwsh", "/src/playwright.ps1", "install", "chromium"]
RUN ["pwsh", "/src/playwright.ps1", "install-deps", "chromium"]
RUN mkdir /ms-playwright-driver
RUN mkdir /ms-playwright-driver/.playwright
COPY --from=sdk /src_playwright/bin/Release/net*/.playwright /ms-playwright-driver/.playwright
ENV PLAYWRIGHT_DRIVER_SEARCH_PATH=/ms-playwright-driver
RUN chmod +x /ms-playwright-driver/.playwright/node/*/node
RUN rm -rf /src/*
