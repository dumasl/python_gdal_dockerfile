FROM centos:centos7

ENV GDAL_VERSION=2.2.1
ENV GDAL_VERSION_PYTHON=2.2.1
ENV PYTHON_VERSION=2.7.15

#
# Basic steps
#
RUN yum install -y epel-release*

RUN yum update -y

RUN yum install -y wget gcc gcc-c++ make which unzip

RUN mkdir /root/COTS

ENV COTS=/root/COTS

#
# PYTHON PART
#
# python and dependencies
#
RUN yum install -y python-devel ncurses-devel libsqlite3x-devel bzip2-devel readline-devel openssl-devel libdb-devel tcl-devel tk-devel 

# python itself
#
RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz -O /tmp/Python-${PYTHON_VERSION}.tar.gz && \
    tar -xzvf /tmp/Python-${PYTHON_VERSION}.tar.gz -C /tmp && \
    cd /tmp/Python-${PYTHON_VERSION} && \
    ./configure --prefix=${COTS}/python${PYTHON_VERSION} --enable-shared --enable-loadable-sqlite-extensions --enable-unicode=ucs4 --with-ensurepip=install && \
    make -j "$(nproc)" && make install
#--enable-optimizations commented for debug purpose 

ENV PYTHON=${COTS}/python${PYTHON_VERSION}/bin/python

# setuptools then pip for this python
#
#RUN wget https://files.pythonhosted.org/packages/1a/04/d6f1159feaccdfc508517dba1929eb93a2854de729fa68da9d5c6b48fa00/setuptools-39.2.0.zip -O /tmp/setuptools-39.2.0.zip && \
#    cd /tmp/ && unzip setuptools-39.2.0.zip && \
#    cd /tmp/setuptools-39.2.0 && $PYTHON setup.py install

#
# GDAL PART
#
RUN yum install -y hdf-devel hdf5-devel libtiff-devel libgeotiff-devel geos-devel postgresql-devel proj-devel poppler-devel jasper-devel curl-devel libpng-devel
RUN wget http://download.osgeo.org/gdal/$GDAL_VERSION/gdal-${GDAL_VERSION}.tar.gz -O /tmp/gdal-${GDAL_VERSION}.tar.gz && \
    tar -x -f /tmp/gdal-${GDAL_VERSION}.tar.gz -C /tmp

RUN cd /tmp/gdal-${GDAL_VERSION} && \
    ./configure \
        --prefix=${COTS}/gdal${GDAL_VERSION} \
        --enable-shared \
        --with-python=${COTS}/python${PYTHON_VERSION}/bin/python \
        --with-geos \
        --with-geotiff \
        --with-jpeg \
        --with-png \
        --with-expat \
        --with-libkml \
        --with-openjpeg \
        --with-curl && \
    make -j "$(nproc)" && make install

RUN rm /tmp/gdal-${GDAL_VERSION} -rf

ENV PATH=${COTS}/gdal${GDAL_VERSION}/bin:$PATH
ENV LD_LIBRARY_PATH=${COTS}/gdal${GDAL_VERSION}/lib:$LD_LIBRARY_PATH
ENV PYTHONPATH=${COTS}/gdal${GDAL_VERSION}/lib64/python2.7/site-packages/:$PYTHONPATH

#
# BASIC PYTHON MODULES
#
ENV PATH=${COTS}/python${PYTHON_VERSION}/bin:$PATH
ENV LD_LIBRARY_PATH=${COTS}/python${PYTHON_VERSION}/lib:$LD_LIBRARY_PATH
ENV PYTHONPATH=${COTS}/python/${PYTHON_VERSION}/lib/python2.7/site-packages/:$PYTHONPATH

RUN easy_install pip && pip install --upgrade pip
RUN pip install virtualenv && \
    pip install numpy && \
    pip install matplotlib && \
    pip install scipy && \
    pip install pyproj

#CMD ["python2"]
