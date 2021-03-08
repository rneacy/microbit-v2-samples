arg FUNCTION_DIR="/home/app/"

FROM python:buster as build-img

run apt-get update && \
  apt-get install -y \
  g++ \
  make \
  cmake \
  unzip \
  libcurl4-openssl-dev 

arg FUNCTION_DIR
run mkdir -p ${FUNCTION_DIR}
copy app/* ${FUNCTION_DIR}

run pip install awslambdaric --target ${FUNCTION_DIR} 

FROM python:buster

#RUN apt-get update -qq && \
#    apt-get install -y --no-install-recommends \
#      software-properties-common && \
#    add-apt-repository -y ppa:team-gcc-arm-embedded/ppa && \
#    apt-get update -qq && \
#    apt-get install -y --no-install-recommends \
#      git make cmake python3 \
#      gcc-arm-embedded && \
#    apt-get autoremove -y && \
#    apt-get clean -y && \
#    rm -rf /var/lib/apt/lists/*

arg FUNCTION_DIR
workdir ${FUNCTION_DIR}

copy --from=build-img ${FUNCTION_DIR} ${FUNCTION_DIR}

#add aws-lambda-rie /usr/local/bin/aws-lambda-rie
ADD https://github.com/aws/aws-lambda-runtime-interface-emulator/releases/latest/download/aws-lambda-rie /usr/bin/aws-lambda-rie

RUN chmod 755 /usr/bin/aws-lambda-rie

copy start.sh /
entrypoint [ "/start.sh" ]

cmd ["app.handler"]
