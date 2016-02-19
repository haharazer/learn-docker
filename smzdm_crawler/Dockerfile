FROM python:2

RUN mkdir -p /var/python/spider
WORKDIR /var/python/spider

COPY requirements.txt /var/python/spider
RUN pip install --no-cache-dir -r requirements.txt -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

# Need hg to download devcron
ADD sources.list /etc/apt/sources.list
RUN apt-get update && apt-get install -y mercurial git

# Yay devcron
RUN pip install -e hg+https://bitbucket.org/dbenamy/devcron#egg=devcron

# Setup defaults
RUN mkdir /cron
ADD crontab /cron/crontab

#checkout code
RUN mkdir -p /tmp && cd /tmp \
    && git clone https://github.com/haharazer/layifa.git \
    && cp -r ./layifa/laiyifaspider/* /var/python/spider

RUN mkdir -p /var/python/spider/pics

CMD ["devcron", "/cron/crontab"]
