ARG APP_HOME=/opt/emon433/

# ============================================================================ #
# Development stage: used only to create the python virtual environment        #
# ============================================================================ #
FROM arm32v7/python:alpine3.9 AS development

LABEL maintainer=""

ARG APP_HOME

# Install required packages
RUN apk add --no-cache git && \
    pip3 install --no-cache-dir virtualenv && \
    mkdir -p ${APP_HOME} && \
    cd ${APP_HOME} && \
    virtualenv venv && \
    . venv/bin/activate && \
    pip3 install --no-cache-dir paho-mqtt configobj pyserial requests influxdb && \
    git clone https://github.com/lmssdd/emonhub.git

# ============================================================================ #
# Final stage: lean, intended to be used as execute container                  #
# docker build --target=final -f docker/Dockerfile \                           #
#              -t tdmproject/edge-emon433-handler .                            #
# ============================================================================ #
FROM arm32v7/python:alpine3.9 AS final

ARG APP_HOME

ENV APP_HOME=${APP_HOME}

COPY --from=development ${APP_HOME} ${APP_HOME}

WORKDIR ${APP_HOME}

#RUN apk add --no-cache git && \
#    git clone https://github.com/lmssdd/emonhub.git

#COPY src src

#COPY conf conf

#EXPOSE 5000

ENTRYPOINT ["emonhub/src/entrypoint.sh"]

#CMD [ "-c", "/opt/configs/tdm.conf" ]

# ============================================================================ #
# Testing Stage: final stage for testing purposes                              #
# docker build --target=testing -f docker/Dockerfile \                         #
#              -t tdmproject/edge-emon433-handler .                            #
# ============================================================================ #
#FROM final AS testing

#ARG APP_HOME

#ENV APP_HOME=${APP_HOME}

#WORKDIR ${APP_HOME}

#COPY tests tests
