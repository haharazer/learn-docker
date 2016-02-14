FROM python:2

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY requirements.txt /usr/src/app/
RUN pip install --no-cache-dir -r requirements.txt -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

#ONBUILD COPY . /usr/src/app

RUN mkdir -p /usr/src/app/laiyifaspider/pics

# Need hg to download devcron
ADD sources.list /etc/apt/sources.list
RUN apt-get update && apt-get install -y mercurial
# Yay devcron
RUN pip install -e hg+https://bitbucket.org/dbenamy/devcron#egg=devcron

# Setup defaults
RUN mkdir /cron
ADD crontab /cron/crontab

VOLUME ["/usr/src/app"]
CMD ["devcron", "/cron/crontab"]
