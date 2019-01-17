#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-sac2016 AS base
WORKDIR /app
EXPOSE 12700
EXPOSE 44366

FROM microsoft/dotnet:2.1-sdk-nanoserver-sac2016 AS build
WORKDIR /src
COPY ["BackendAPI/BackendAPI.csproj", "BackendAPI/"]
RUN dotnet restore "BackendAPI/BackendAPI.csproj"
COPY . .
WORKDIR "/src/BackendAPI"
RUN dotnet build "BackendAPI.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "BackendAPI.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "BackendAPI.dll"]