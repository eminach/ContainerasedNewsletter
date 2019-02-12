#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 52232
EXPOSE 44310

FROM microsoft/dotnet:2.1-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["Customer.API/Customer.API.csproj", "Customer.API/"]
RUN dotnet restore "Customer.API/Customer.API.csproj"
COPY . .
WORKDIR "/src/Customer.API"
RUN dotnet build "Customer.API.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "Customer.API.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Customer.API.dll"]