FROM zburgermeiszter/asterisk:11-with-samples

MAINTAINER Zoltan Burgermeiszter <zoltan@burgermeiszter.com>

ADD etc/asterisk/*.conf /etc/asterisk/
RUN chown -R asterisk:117 /etc/asterisk/ && chmod -R 640 /etc/asterisk