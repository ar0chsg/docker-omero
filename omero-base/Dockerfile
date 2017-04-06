FROM ubuntu:16.04

MAINTAINER hadrien.mary@gmail.com

RUN sed 's/main$/main universe/' -i /etc/apt/sources.list

# Install dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    unzip wget supervisor \
    python2.7 python-pil python-matplotlib python-pip \
    python-numpy python-tables python-scipy \
    openjdk-8-jre-headless \
    mencoder nano postgresql-client postgresql-client-common

RUN apt-get -y install db5.3-util
RUN apt-get -y install libssl-dev libbz2-dev libmcpp-dev libdb++-dev libdb-dev
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 5E6DA83306132997
RUN apt-get install -y software-properties-common
RUN apt-add-repository "deb http://zeroc.com/download/apt/ubuntu`lsb_release -rs` stable main"
RUN apt-get update
RUN apt-get -y install zeroc-ice-all-runtime zeroc-ice-all-dev
RUN pip install "zeroc-ice>3.5,<3.7"


ENV BRANCH 5.3.0
ENV ICE 3.6

# Path variables
ENV OMERO_DIR /omero
ENV OMERO_HOME $OMERO_DIR/OMERO.server

# Add omero user and install omego tools
RUN useradd -m omero
RUN pip install -U pip
RUN pip install -U omego

RUN mkdir $OMERO_DIR
RUN chown omero $OMERO_DIR

# Download Omero with omego
USER omero
WORKDIR $OMERO_DIR

RUN omego download --branch=$BRANCH --ice $ICE --unzipdir . -v server
RUN rm OMERO.server*zip

RUN ln -s OMERO.server-* $OMERO_HOME

USER root