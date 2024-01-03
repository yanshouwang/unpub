# Use latest stable channel SDK.
FROM dart:stable AS build

# Resolve app dependencies.
WORKDIR /app
COPY pubspec.* ./
RUN dart pub get

# Copy app source code (except anything in .dockerignore) and AOT compile app.
COPY . .
RUN dart compile exe bin/unpub.dart -o bin/unpub

# Build minimal serving image from AOT-compiled `/unpub`
# and the pre-built AOT-runtime in the `/runtime/` directory of the base image.
FROM scratch
COPY --from=build /runtime/ /
COPY --from=build /app/bin/unpub /app/bin/

# Start unpub.
EXPOSE 4000
CMD ["/app/bin/unpub", "--database", "mongodb://$UNPUB_DB_HOST:27017/dart_pub"]
