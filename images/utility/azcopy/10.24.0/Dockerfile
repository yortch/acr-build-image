FROM alpine:3.19 as build
WORKDIR /azcopy
RUN apk add --no-cache wget \
 && wget https://azcopyvnext.azureedge.net/releases/release-10.24.0-20240326/azcopy_linux_amd64_10.24.0.tar.gz -O azcopy.tar.gz \
 && tar -xzf azcopy.tar.gz --strip=1


FROM alpine:3.19 as release
ARG AZCOPY_VERSION
LABEL name="azcopy"
LABEL version=10.24.0
RUN apk update && apk add libc6-compat
COPY --from=build /azcopy/azcopy /usr/local/bin/azcopy
CMD [ "azcopy" ]
