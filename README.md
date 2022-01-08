# docker-qt5-builder

This docker container facilitates the compilation of qt5 based applications. Can be used either as a base image to 
compile applications for use within other docker containers, or independently via a docker run command to compile and
output binaries to a host system.

## Examples

These examples illustrate how to compile [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) with the use of this docker container.

**Docker run**

```sh
docker run \
  --rm \
  -v /build:/build \
  -v /src:/src \
  thelolagemann/qt5-builder:latest \
  sh -c "qmake /src/OpenRGB.pro && make -j$(nproc)"
```

<sup>*Assuming the OpenRGB source files are already present in the `/src` directory*</sup>

**Dockerfile**

```dockerfile
FROM thelolagemann/qt5-builder:latest as builder

RUN git clone "https://gitlab.com/CalcProgrammer1/OpenRGB.git"
RUN qmake "/build/OpenRGB/OpenRGB.pro"
RUN make -j$(nproc)

FROM alpine:3.15

COPY --from=builder /build/openrgb /usr/bin/openrgb
CMD ["openrgb"]
```

<sup>*Additional setup steps pertaining to OpenRGB have been omitted for the sake of brevity*</sup>
