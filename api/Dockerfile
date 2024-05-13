FROM python:3.8.19

# Setup a spot for the api code
WORKDIR /usr/local/src/skiff/app/api

ARG GITHUB_ACCESS_TOKEN

# Install Python dependencies
RUN pip install --upgrade pip setuptools wheel
COPY requirements.txt .

RUN pip install -r requirements.txt

# Install Java
RUN apt-get update && \
    apt-get install -y python3-launchpadlib --no-install-recommends software-properties-common &&\
    add-apt-repository -y ppa:openjdk-r/ppa

RUN apt-get update && \
    mkdir -p /usr/share/man/man1/ && \
    apt-cache search jdk && \
    apt-get install -y openjdk-17-jdk && \
    apt-get install -y openjdk-17-jre && \
    update-alternatives --config java && \
    update-alternatives --config javac 

# Set JAVA_HOME environment variable
ENV JAVA_HOME /usr/lib/jvm/java-17-openjdk-amd64

# Copy over the source code
COPY app app/
COPY config config/
COPY main.py main.py

# Kick things off
ENTRYPOINT [ "uvicorn" ]
CMD ["main:app", "--host", "0.0.0.0"]
