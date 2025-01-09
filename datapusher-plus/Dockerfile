#############################
### Build DataPusher Plus ###
#############################

# See example Dockerfile for datapusher+ for more details (https://github.com/dathere/datapusher-plus-docker)
FROM python:3.10-bookworm

# Install apt-utils and other dependencies
RUN apt-get update && apt-get install --no-install-recommends -y \
    libxslt1-dev \
    libxml2-dev \
    libffi-dev \
    unzip \
    libpq-dev \
    file

# Clean up APT when done
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install qsv;
ADD https://github.com/jqnatividad/qsv/releases/download/0.108.0/qsv-0.108.0-x86_64-unknown-linux-gnu.zip ./qsv.zip
RUN unzip qsv.zip && rm qsv.zip && mv qsv* /usr/local/bin

# Install uwsgi;
RUN pip3 install -U uwsgi

# Install datapusher-plus;
RUN pip3 install datapusher-plus==0.16.4

# Setup datapusher-plus;
RUN useradd -m -s /bin/bash app

USER app

WORKDIR /home/app

COPY .env ./.env
ADD https://raw.githubusercontent.com/dathere/datapusher-plus/0.16.4/deployment/datapusher-uwsgi.ini ./uwsgi.ini

EXPOSE 8800

ENV JOB_CONFIG=/home/app/.env

CMD [ "uwsgi", "--socket=/tmp/uwsgi.sock", "--enable-threads", "-i", "/home/app/uwsgi.ini", "--wsgi-file=datapusher_plus.wsgi" ]