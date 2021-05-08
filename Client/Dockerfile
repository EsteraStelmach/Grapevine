#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build-env
# Setting the work directory for our app
WORKDIR /Grapevine2

# We copy the .csproj of our app to root and 
# restore the dependencies of the project.
COPY /Grapevine.csproj .
RUN dotnet restore "Grapevine.csproj"

# We proceed by copying all the contents in
# the main project folder to root and build it
COPY / .
RUN dotnet build "Grapevine.csproj" -c Release -o /build




# Once we're done building, we'll publish the project
# to the publish folder 
#FROM build-env AS publish
RUN dotnet publish "Grapevine.csproj" -c Release -o /publish




# We then get the base image for Nginx and set the 
# work directory 
FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html

# We'll copy all the contents from wwwroot in the publish
# folder into nginx/html for nginx to serve. The destination
# should be the same as what you set in the nginx.conf.
COPY --from=build-env /publish/wwwroot /usr/local/webapp/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
