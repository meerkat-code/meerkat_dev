FROM python:3.7

RUN apt-get update

# Install global requirements
ENV PYTHONPATH=/var/www/meerkat_libs
RUN wget -qO- https://deb.nodesource.com/setup_10.x | sed 's/apt-get update/apt-get -o Acquire::Check-Valid-Until=false update/' | bash - &&\
    apt-get install -y nodejs gettext ghostscript &&\
    apt-get clean
RUN npm install -g bower gulp-cli yarn
WORKDIR /var/www

RUN ln -s /usr/bin/python3 /usr/bin/python & \
    ln -s /usr/bin/pip3 /usr/bin/pip

# Install pip requirements. Add requirements list to bust-cache if any changes.
ADD meerkat_libs/requirements.txt libs_req.tmp
RUN pip install -r libs_req.tmp && rm libs_req.tmp
ADD meerkat_frontend/requirements.txt frontend_req.tmp
RUN pip install -r frontend_req.tmp && rm frontend_req.tmp

# Install dependancies and configs.
# requirements.txt most likely to change so put it last
ADD meerkat_dev/frontend/uwsgi/app.ini /var/www/uwsgi/app.ini
ADD meerkat_dev/frontend/setup_static /usr/bin/setup_static
ADD meerkat_dev/frontend/run /usr/bin/run
WORKDIR /var/www/meerkat_frontend

CMD ["run"]
