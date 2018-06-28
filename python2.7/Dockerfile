FROM python:2.7.15

ENV GDAL_VERSION=2.2.1
ENV GDAL_VERSION_PYTHON=2.2.1

RUN apt-get update

RUN apt-get -y install wget

RUN apt-get -y install libcurl4-openssl-dev

RUN apt-get -y install build-essential

RUN apt-get -y install libpq-dev

RUN apt-get -y install ogdi-bin

RUN apt-get -y install libogdi3.2-dev

RUN apt-get -y install libjasper-runtime

RUN apt-get -y install libjasper-dev

RUN apt-get -y install libjasper1

RUN apt-get -y install libgeos-c1v5

RUN apt-get -y install libproj-dev

RUN apt-get -y install libpoppler-dev

RUN apt-get -y install libsqlite3-dev

RUN apt-get -y install libspatialite-dev

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
