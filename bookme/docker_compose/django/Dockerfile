FROM debian:stable

ENV LANG C.UTF-8
ENV PYTHONUNBUFFERED 1

RUN apt-get update && apt-get install -y \
    git \
    virtualenv \
    python3-dev \
    build-essential \
    libgeos-c1v5 \
    libpq-dev \
    libproj-dev \
    libfreexl-dev \
    libgdal-dev \
    gdal-bin\
    python3-pip

#FROM python:3.6.2

# update package lists, fix broken system packages
#RUN apt-get update -y
RUN apt-get -f install

# install and cache dependencies in /tmp directory.
# doing it this way also installs any newly added dependencies.
RUN pip3 install --upgrade pip
ADD requirements.txt /tmp/requirements.txt
RUN pip3 install -r /tmp/requirements.txt

# load project files and set work directory
ADD . /app/
WORKDIR /app

# create user and add to docker group
RUN adduser --disabled-password --gecos '' djangobookme
RUN groupadd docker
RUN usermod -aG docker djangobookme

# grant newly created user permissions on essential files
RUN chown -R djangobookme:$(id -gn djangobookme) ~/
RUN chown -R djangobookme:$(id -gn djangobookme) /app/

# change user to newly created user
USER djangobookme
