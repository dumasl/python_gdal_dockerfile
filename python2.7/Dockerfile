FROM python:2.7.15

ENV GDAL_VERSION=2.2.1
ENV GDAL_VERSION_PYTHON=2.2.1

RUN apt-get update && \
    apt-get -y install \
        wget \
        libcurl4-openssl-dev \
        build-essential \
        libpq-dev \
        ogdi-bin \
        libogdi3.2-dev \
        libjasper-runtime \
        libjasper-dev \
        libjasper1 \
        libgeos-c1v5 \
        libproj-dev \
        libpoppler-dev \
        libsqlite3-dev \
        libspatialite-dev

RUN wget http://download.osgeo.org/gdal/$GDAL_VERSION/gdal-${GDAL_VERSION}.tar.gz -O /tmp/gdal-${GDAL_VERSION}.tar.gz && \
    tar -x -f /tmp/gdal-${GDAL_VERSION}.tar.gz -C /tmp

RUN cd /tmp/gdal-${GDAL_VERSION} && \
    ./configure \
        --prefix=/usr/local/gdal_${GDAL_VERSION} \
        --with-python \
        --with-geos \
        --with-geotiff \
        --with-jpeg \
        --with-png \
        --with-expat \
        --with-libkml \
        --with-openjpeg \
        --with-pg \
        --with-curl \
        --with-spatialite && \
    make && make install

RUN rm /tmp/gdal-${GDAL_VERSION} -rf

#RUN pip install GDAL==${GDAL_VERSION_PYTHON}

CMD ["python2"]
