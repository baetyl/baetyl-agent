FROM --platform=$TARGETPLATFORM baetyltech/gocv-devel:v0.24.0 as devel
COPY * /root/go/baetyl-video-infer/
RUN cd /root/go/baetyl-video-infer/ && \
    make build

FROM --platform=$TARGETPLATFORM debian:buster

RUN apt-get update -y \
 && apt-get install -y --no-install-recommends pkg-config libgtk2.0-dev libavcodec-dev libavformat-dev libswscale-dev libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev \
 && rm -rf /var/lib/apt/lists/*

# OpenCV
COPY --from=devel /usr/local/lib /usr/local/lib
COPY --from=devel /usr/local/lib/pkgconfig/opencv4.pc /usr/local/lib/pkgconfig/opencv4.pc
COPY --from=devel /usr/local/include/opencv4/opencv2 /usr/local/include/opencv4/opencv2
ENV PKG_CONFIG_PATH /usr/local/lib/pkgconfig
ENV LD_LIBRARY_PATH /usr/local/lib

# baetyl-video-infer
COPY --from=devel /root/go/baetyl-video-infer/output/baetyl-video-infer /bin/

ENTRYPOINT ["baetyl-video-infer"]