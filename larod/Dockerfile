# Specify the architecture at build time: armv7hf/aarch64
# Should be used for getting API image
# Currently, only armv7hf is supported
ARG ARCH=armv7hf
ARG VERSION=3.1
ARG UBUNTU_VERSION=19.10

FROM axisecp/acap-api:${VERSION}-${ARCH}-ubuntu${UBUNTU_VERSION} as api
FROM axisecp/acap-toolchain:${VERSION}-${ARCH}-ubuntu${UBUNTU_VERSION} as toolchain

# Use PKG_ARCH=aarch64-poky-linux if ARCH=aarch64
ARG PKG_ARCH=cortexa9hf-neon-poky-linux-gnueabi

# Copy the lib, include and .pc files from the API container
COPY --from=api /opt/axis/sdk/temp/sysroots/${PKG_ARCH}/usr/lib/ /opt/axis/sdk/sysroots/${PKG_ARCH}/usr/lib/
COPY --from=api /opt/axis/sdk/temp/sysroots/${PKG_ARCH}/usr/include/ /opt/axis/sdk/sysroots/${PKG_ARCH}/usr/include/
COPY --from=api /opt/axis/sdk/temp/sysroots/${PKG_ARCH}/usr/lib/pkgconfig/ /opt/axis/sdk/sysroots/${PKG_ARCH}/usr/lib/pkgconfig/

# Copy the tools and scripts from the toolchain container
RUN cp -r /opt/axis/sdk/temp/* /opt/axis/sdk/

# Install any non-axis package that your build process/ACAP needs
RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        xz-utils \
        iputils-ping

# The building of the particular ACAP
COPY ./app /opt/app/
WORKDIR /opt/app
RUN . /opt/axis/sdk/environment-setup* && create-package.sh