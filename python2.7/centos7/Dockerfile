FROM ldumas/centos7_dockerfile:centos7

ENV GDAL_VERSION=2.2.1
ENV GDAL_VERSION_PYTHON=2.2.1
ENV PYTHON_VERSION=2.7.15

ENV COTS=/opt/

# save initial env state
ENV INIT_PATH=$PATH
ENV INIT_LD_LIBRARY_PATH=$PATH
ENV INIT_PYTHONPATH=$PYTHONPATH

# python dependencies
#
RUN yum install -y python-devel ncurses-devel libsqlite3x-devel bzip2-devel readline-devel openssl-devel libdb-devel tcl-devel tk-devel 

# gdal dependencies
#
RUN yum install -y hdf-devel hdf5-devel libtiff-devel libgeotiff-devel geos-devel postgresql-devel proj-devel poppler-devel jasper-devel curl-devel libpng-devel

# python itself
#
RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz -O /tmp/Python-${PYTHON_VERSION}.tar.gz && \
    tar -xzvf /tmp/Python-${PYTHON_VERSION}.tar.gz -C /tmp && \
    cd /tmp/Python-${PYTHON_VERSION} && \
    ./configure --prefix=${COTS}/python${PYTHON_VERSION} --enable-shared --enable-loadable-sqlite-extensions --enable-unicode=ucs4 --with-ensurepip=install --enable-optimizations && \
    make -j "$(nproc)" && make install

ENV PYTHON=${COTS}/python${PYTHON_VERSION}/bin/python

# python required modules
#
ENV PY_PATH=${COTS}/python${PYTHON_VERSION}/bin
ENV PY_LD_PATH=${COTS}/python${PYTHON_VERSION}/lib
ENV PY_PYTHONPATH=${COTS}/python/${PYTHON_VERSION}/lib/python2.7/site-packages/

ENV PATH=${PY_PATH}:$PATH
ENV LD_LIBRARY_PATH=${PY_LD_PATH}:$LD_LIBRARY_PATH
ENV PYTHONPATH=${PY_PYTHONPATH}:$PYTHONPATH

RUN easy_install pip && pip install --upgrade pip
RUN pip install virtualenv && \
    pip install numpy && \
    pip install matplotlib && \
    pip install scipy && \
    pip install pyproj


# GDAL 
#
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

ENV PY_PATH=${COTS}/gdal${GDAL_VERSION}/bin:${PY_PATH}
ENV PY_LD_PATH=${COTS}/gdal${GDAL_VERSION}/lib:${PY_LD_PATH}
ENV PY_PYTHONPATH=${COTS}/gdal${GDAL_VERSION}/lib/python2.7/site-packages/:${PY_PYTHONPATH}

ENV PATH=${PY_PATH}:$PATH
ENV LD_LIBRARY_PATH=${PY_LD_PATH}:$LD_LIBRARY_PATH
ENV PYTHONPATH=${PY_PYTHONPATH}:$PYTHONPATH


# MIGHT BE USEFUL METHODS FOR SETTING UP ENV
#
RUN echo -e '#!/bin/bash\nexport PATH=${INIT_PATH} && export LD_LIBRARY_PATH=${INIT_LD_LIBRARY_PATH} && export PYTHONPATH=${INIT_PYTHONPATH}' > /usr/bin/restart_env && \
    chmod +x /usr/bin/restart_env
RUN echo -e '#!/bin/bash\nexport export PATH=${PY_PATH}:$PATH && export LD_LIBRARY_PATH=${PY_LD_PATH}:${LD_LIBRARY_PATH} && export PYTHONPATH=${PY_PYTHONPATH}:$PYTHONPATH' > /usr/bin/pygdal_env && \
    chmod +x /usr/bin/pygdal_env

ENV OPT_GDAL_DIR=${COTS}/gdal${GDAL_VERSION}
ENV OPT_PYTHON_DIR=${COTS}/python${PYTHON_VERSION}


#CMD ["python2"]
