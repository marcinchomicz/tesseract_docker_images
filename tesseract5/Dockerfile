FROM ubuntu:20.04
LABEL maintainer="marcin.chomicz@mch.net.pl"

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y mc

# tutaj jest instalacja wersji 5 ze źródeł
RUN DEBIAN_FRONTEND=noninteractive apt -y install automake ca-certificates g++ git libtool libleptonica-dev make pkg-config libpango1.0-dev
RUN DEBIAN_FRONTEND=noninteractive git clone https://github.com/tesseract-ocr/tesseract.git
WORKDIR /tesseract
RUN DEBIAN_FRONTEND=noninteractive ./autogen.sh
RUN DEBIAN_FRONTEND=noninteractive ./configure
RUN make && make install
RUN ldconfig
RUN make training && make training install

# instalacja pythona
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y software-properties-common
RUN DEBIAN_FRONTEND=noninteractive add-apt-repository -y ppa:deadsnakes/ppa
RUN DEBIAN_FRONTEND=noninteractive apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install python3.8
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install pkg-config
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libcairo2-dev libjpeg-dev libgif-dev
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install ffmpeg libsm6 libxext6  -y

RUN DEBIAN_FRONTEND=noninteractive apt-get -y install python3-pip
RUN DEBIAN_FRONTEND=noninteractive pip install virtualenv
RUN DEBIAN_FRONTEND=noninteractive /usr/bin/python3.8 -m pip install --upgrade pip
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install nodejs

COPY ./tesseract5/requirements.txt /opt/requirements.txt
RUN DEBIAN_FRONTEND=noninteractive pip install -Ur /opt/requirements.txt --prefer-binary

COPY ./tessdata/pol.traineddata /usr/local/share/tessdata/pol.traineddata
RUN mkdir /home/images_in
RUN mkdir /home/images_out

EXPOSE 8888/tcp
EXPOSE 8888/udp
CMD jupyter lab --allow-root --ip 0.0.0.0 --notebook-dir=/ --ServerApp.token='' --ServerApp.password=''

# Budujemy z kontekstu docker build -t tesseract:5 --progress=plain  --no-cache -f tesseract5\Dockerfile .
