FROM lgse/gocv-alpine:4.1.2-buildstage

ENV PKG="libstdc++ \
         ca-certificates \
         wget \
         libjpeg-turbo \
         libpng \
         libwebp \
         libwebp-dev \
         tiff \
         openblas \
         libgphoto2"

RUN apk update && \
    apk upgrade && \
    apk add --no-cache ${PKG} && \
    # The gblic v2.28-r0 doesn't have sgerrand.rsa.pub.
    # Downloading it from previous version.
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.27-r0/sgerrand.rsa.pub && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.28-r0/glibc-2.28-r0.apk && \
    apk add --no-cache glibc-2.28-r0.apk && \
    rm -fr /glibc-2.28-r0.apk && \
    rm -rf /var/cache/apk/* && \
    apk del wget

ENV PKG_CONFIG_PATH /usr/local/lib64/pkgconfig
ENV LD_LIBRARY_PATH /usr/local/lib64
ENV CGO_CPPFLAGS -I/usr/local/include
ENV CGO_CXXFLAGS "--std=c++1z"
ENV CGO_LDFLAGS "-L/usr/local/lib -lopencv_core -lopencv_face -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs -lopencv_objdetect -lopencv_features2d -lopencv_dnn -lopencv_xfeatures2d -lopencv_plot -lopencv_tracking"
