FROM ubuntu:20.04

LABEL maintainer="Ascensio System SIA <support@onlyoffice.com>"

ARG VERSION="0.4"
ARG RELEASE="8"
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update && \
    apt-get -yq install gnupg2 && \
    echo "deb http://download.mono-project.com/repo/ubuntu stable-focal main" | tee /etc/apt/sources.list.d/mono-official.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    apt-get -y update && \
    apt-get install -yq mono-complete alien git autoconf automake libtool make libglib2.0-dev libevent-dev libtool-bin && \
    echo "Build HyperFastCGI" && \
    git clone https://github.com/ONLYOFFICE/HyperFastCgi.git && \
    cd HyperFastCgi && \
    mkdir -p rpmbuild/BUILD rpmbuild/RPMS rpmbuild/SOURCES rpmbuild/SPECS rpmbuild/SRPMS && \
    git archive --format=tar --prefix=mono-webserver-hyperfastcgi-${VERSION}-${RELEASE}/ HEAD > rpmbuild/SOURCES/mono-webserver-hyperfastcgi-${VERSION}-${RELEASE}.tar && \
    DESTDIR="/HyperFastCgi/rpmbuild/BUILDROOT/mono-webserver-hyperfastcgi-${VERSION}-${RELEASE}.x86_64" rpmbuild -bb --define "_topdir /HyperFastCgi/rpmbuild" \
    --define "package_header_tag_version ${VERSION}" \
    --define "package_header_tag_release ${RELEASE}" \
    hyperfastcgi.spec && \
    alien -k /HyperFastCgi/rpmbuild/RPMS/noarch/mono-webserver-hyperfastcgi-${VERSION}-${RELEASE}.noarch.rpm
