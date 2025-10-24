# Build Multiplatform
```bash
docker buildx create --name multiarch --use
docker build inspect --bootstrap
docker buildx build --platform linux/amd64, linux/arm64 \                    30m 6s 11:17:12 PM
-t asoltes/liqubase:v1.0.0-beta --push
```