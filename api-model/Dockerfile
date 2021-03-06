FROM python:3.7

RUN apt-get update
RUN apt-get install -y --no-install-recommends libatlas-base-dev gfortran nginx supervisor

COPY conf/requirements.txt /app/requirements.txt

RUN pip3 install --upgrade pip
RUN pip3 install -r /app/requirements.txt

RUN useradd --no-create-home nginx

RUN rm /etc/nginx/sites-enabled/default
RUN rm -r /root/.cache

COPY conf/nginx.conf /etc/nginx/
COPY conf/flask-nginx.conf /etc/nginx/conf.d/
COPY conf/uwsgi.ini /etc/uwsgi/
COPY conf/supervisord.conf /etc/supervisor/

COPY ./src /app/src

RUN mkdir -p /app/data
COPY ./data /app/data

RUN chown -R nginx /app
RUN chmod -R 777 /app/data

WORKDIR /app

EXPOSE 5000

CMD ["/usr/bin/supervisord"]