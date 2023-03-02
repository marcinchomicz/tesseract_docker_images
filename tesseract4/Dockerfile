FROM ubuntu:20.04
LABEL maintainer="marcin.chomicz@mch.net.pl"

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y mc

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y tesseract-ocr
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libtesseract-dev

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

COPY ./tesseract4/requirements.txt /opt/requirements.txt
RUN DEBIAN_FRONTEND=noninteractive pip install -Ur /opt/requirements.txt --prefer-binary

COPY ./tessdata/pol.traineddata /usr/share/tesseract-ocr/4.00/tessdata/pol.traineddata
RUN mkdir /home/images_in
RUN mkdir /home/images_out

EXPOSE 8888/tcp
EXPOSE 8888/udp
CMD jupyter lab --allow-root --ip 0.0.0.0 --notebook-dir=/ --ServerApp.token='' --ServerApp.password=''

# Budujemy z kontekstu docker build -t tesseract:4 --progress=plain  --no-cache -f tesseract4\Dockerfile .

# uruchamiamy na przykład tak:
#docker run -it -v S:\_WORK_\tesseract_docker_images\tesseract4\images_in:/home/images_in
#-v S:\_WORK_\tesseract_docker_images\tesseract4\images_out:/home/images_out -p 8888:8888 tesseract:4
