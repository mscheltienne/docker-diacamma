FROM python:3.9-slim-bullseye as builder

ENV PACKAGES lucterios lucterios-standard lucterios-contacts lucterios-documents diacamma-asso diacamma-syndic diacamma-financial
ENV APP_NAME Diacamma
ENV LUCTERIOS_PATH /var/lucterios2

RUN set -x ; \
  apt update ; \
  apt-get install -y libxml2-dev libxslt-dev libjpeg-dev libfreetype6 libfreetype6-dev zlib1g-dev \
	 python3-pip python3-dev 

# ENV PIP_OPTION ''
# ENV PIP_OPTION --proxy=$http_proxy

RUN set -x ; \
  python3 -m pip install -U $PIP_OPTION pip==21.3.* virtualenv

WORKDIR $LUCTERIOS_PATH

RUN set -x ; \
  mkdir -p $LUCTERIOS_PATH ; \

RUN set -x ; \
  rm -rf virtual_for_lucterios ; \
  python3 -m virtualenv virtual_for_lucterios

RUN set -x ; \
  . $LUCTERIOS_PATH/virtual_for_lucterios/bin/activate ; \
  pip install -U $PIP_OPTION pip ; \
  pip install -U $PIP_OPTION $PACKAGES

RUN set -x ; \
  . $LUCTERIOS_PATH/virtual_for_lucterios/bin/activate ; \
  pip install gunicorn

RUN set -x ; \
  . $LUCTERIOS_PATH/virtual_for_lucterios/bin/activate ; \
  pip install psycopg2-binary

# RUN set -x ; \
#   . $LUCTERIOS_PATH/virtual_for_lucterios/bin/activate ; \
#   pip install mysqlclient

ADD launch_lucterios.sh $LUCTERIOS_PATH
ADD entrypoint.d /entrypoint.d

RUN ls /entrypoint.d


EXPOSE 8100
ENTRYPOINT /entrypoint.d/entrypoint.sh run

