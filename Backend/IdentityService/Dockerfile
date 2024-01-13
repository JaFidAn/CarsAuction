FROM mcr.microsoft.com/dotnet/sdk:8.0 as build
WORKDIR /app
EXPOSE 80

#copy all .csproj files and restore as distinct layers. Use of the same COPY command for every dockerfile in the project to take advantage of docker caching
COPY carsauction.sln carsauction.sln
COPY Backend/AuctionService/AuctionService.csproj Backend/AuctionService/AuctionService.csproj
COPY Backend/SearchService/SearchService.csproj Backend/SearchService/SearchService.csproj
COPY Backend/GatewayService/GatewayService.csproj Backend/GatewayService/GatewayService.csproj
COPY Backend/BiddingService/BiddingService.csproj Backend/BiddingService/BiddingService.csproj
COPY Backend/Contracts/Contracts.csproj Backend/Contracts/Contracts.csproj
COPY Backend/IdentityService/IdentityService.csproj Backend/IdentityService/IdentityService.csproj


#Restore package deps
RUN dotnet restore carsauction.sln

#Copy the app folders over
COPY Backend/IdentityService Backend/IdentityService
WORKDIR /app/Backend/IdentityService
RUN dotnet publish -c Release -o /app/src/out

#Build Runtime Image
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/src/out .
ENTRYPOINT [ "dotnet", "IdentityService.dll" ]