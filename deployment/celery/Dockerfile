FROM centos/python-36-centos7

# Add OpenROAD flow tools
USER root
RUN yum install -y tcl libX11-devel libgomp libXft

COPY deployment/requirements.deploy.txt /requirements.txt
RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r /requirements.txt

COPY deployment/celery/tools /openroad/tools/
COPY deployment/celery/lib /openroad/lib/
COPY deployment/celery/tcl /openroad/tcl/
COPY deployment/celery/share /openroad/share/
COPY src /src/
COPY deployment/celery/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

WORKDIR /src

RUN mkdir /playground && chmod 777 /playground && \
    mkdir /strg && chmod 777 /strg

ENV DJANGO_SETTINGS_MODULE=flow.settings
ENV UWSGI_WSGI_FILE=/src/flow/wsgi.py UWSGI_HTTP=:8000 UWSGI_MASTER=1 UWSGI_WORKERS=2 UWSGI_THREADS=8 UWSGI_UID=1000 UWSGI_GID=2000 UWSGI_LAZY_APPS=1 UWSGI_WSGI_ENV_BEHAVIOR=holy
ENV OPENROAD /openroad

EXPOSE 8000
ENTRYPOINT ["/entrypoint.sh"]