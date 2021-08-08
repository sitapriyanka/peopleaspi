FROM microsoft/dotnet:2.2-sdk AS build-env
RUN mkdir /app
WORKDIR /app

# copy the project and restore as distinct layers in the image
COPY ./*.csproj ./
RUN dotnet restore

# copy the rest and build
COPY ./ ./
RUN dotnet build
RUN dotnet publish -c Release -o out

# build runtime image
FROM microsoft/dotnet:2.2-aspnetcore-runtime
RUN mkdir /app
EXPOSE 5000 7000
ENV ASPNETCORE_URLS=http://*:5000
WORKDIR /app

COPY --from=build-env /app/out ./
ENTRYPOINT ["dotnet", "peopleapi.dll"]