### builder
FROM mcr.microsoft.com/dotnet/sdk:5.0-focal-arm64v8 AS builder
ENV DEBIAN_FRONTEND=noninteractive

ARG SCRCPY_VER=1.24
ARG SERVER_HASH="ae74a81ea79c0dc7250e586627c278c0a9a8c5de46c9fb5c38c167fb1a36f056"

RUN apt-get update && apt-get -y install --no-install-recommends \
      apt-transport-https \
      software-properties-common \
      ffmpeg libsdl2-2.0-0 adb libusb-1.0-0 \
      gcc git pkg-config meson ninja-build libsdl2-dev \
        libavcodec-dev libavdevice-dev libavformat-dev libavutil-dev \
        libusb-1.0-0-dev \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/*

RUN PATH=$PATH:/usr/lib/jvm/java-1.8-openjdk/bin
RUN curl -L -o scrcpy-server https://github.com/Genymobile/scrcpy/releases/download/v${SCRCPY_VER}/scrcpy-server-v${SCRCPY_VER}
RUN echo "$SERVER_HASH  /scrcpy-server" | sha256sum -c -

RUN git clone https://github.com/Genymobile/scrcpy.git
RUN ls -l scrcpy
RUN cd scrcpy && meson x --buildtype release --strip -Db_lto=true -Dprebuilt_server=/scrcpy-server

RUN ls -l scrcpy/x 
RUN cd scrcpy/x && ninja

